---
published: true
description: DNS over HTTPS (DoH) and DNS over TLS (DoT) encrypt DNS queries, but they don't fully protect user privacy on their own. Without Encrypted Client Hello (ECH), third parties can still see the destination domain via the unencrypted SNI field in the TLS handshake.
tags:
  - security
  - privacy
title: ECH - The Missing Layer in DNS Privacy
header:
 image: /images/header/header_privacy.jpg
 teaser: /images/header/header_privacy.jpg
---

This week, there was a discussion on [Hacker News](https://news.ycombinator.com/item?id=44215608) about the reasons for and against using DNS over HTTPS (DoH). **DNS over HTTPS (DoH)** and **DNS over TLS (DoT)** are protocols designed to encrypt DNS traffic. 
The legacy DNS protocol — *also known as the phonebook of the internet™* — sends all queries unencrypted, meaning that third parties (e.g., your ISP) can see every domain you request.

My perspective on this discussion: Neither DoH nor DoT significantly improve privacy on their own. While encrypting DNS traffic prevents ISPs or other third parties from seeing the content of your DNS queries, they can still infer which websites you're visiting by observing the **Server Name Indication (SNI)** field, which is part of the TLS handshake and includes the domain name being accessed.

![Screenshot of my Mastodon post where I mention that neither DoH nor DoT significantly contribute to privacy]({{site.baseurl}}/images/mastodon.png)

## RFC for Encrypted Client Hello (ECH)

I was already aware of ongoing efforts to encrypt the SNI as well, which would significantly contribute to privacy when combined with DoH / DoT. [@rmbolger@mastodon.social](https://mastodon.social/@rmbolger/114649609485099260) pointed out that there is an active RFC draft for [**Encrypted Client Hello (ECH)**](https://datatracker.ietf.org/doc/draft-ietf-tls-esni/).
With ECH, the client can encrypt its ClientHello to the TLS server. This includes the SNI field and a few other potentially sensitive elements like the Application Layer Protocol Negotiation (ALPN). The key privacy benefit is that third parties can no longer read the SNI and deduce the destination domain.

To return to DoH and DoT: I believe both protocols would really improve privacy, but only **when combined with ECH**. Therefore, widespread implementation of ECH is highly desirable.

## Testing ECH Support in your setup

I wanted to know if ECH is already implemented, so I did a bit of research. It turns out Firefox [already supports ECH](https://support.mozilla.org/en-US/kb/faq-encrypted-client-hello).

On the server side, implementations are emerging too. For example, OpenSSL has a [feature branch](https://github.com/openssl/openssl/tree/feature/ech) that supports ECH.

Since ECH depends on specific DNS records (notably HTTPS and SVCB) you need to use a DNS server that returns these records. They are mandatory for ECH and contain the public key for the encryption and some other metadata. Firefox users can enable DNS over HTTPS (DoH) to ensure proper resolution, as many default DNS servers do not yet provide HTTPS/SVCB records. I did this for my test, because normally I don't use DoH.

To test whether ECH is working in your setup, visit: [https://test.defo.ie/](https://test.defo.ie/)

If everything is working as expected (assuming you're using Firefox), the site will display **SSL_ECH_STATUS_SUCCESS**. Chrome should also support ECH, though Safari does not as of now.

## Real-World Examples

I also wanted to see how this works beyond the test site, particularly in terms of actual network traffic.

First, I captured TLS traffic in Wireshark without ECH. In this scenario, the SNI remains unencrypted, and the Server Name field displays the domain name mialikescoffee.com

![Screenshot of Wireshark. The SNI field shows mialikescoffee.com]({{site.baseurl}}/images/sni_1.png)

Next, I looked at research.cloudflare.com, which supports ECH. In this capture, the Server Name Indicator is shown as cloudflare-ech.com, and the packet capture includes the Extension: encrypted_client_hello — confirming that ECH is working properly. In this case, a third party cannot determine which specific domain is being accessed.

![Screenshot of Wireshark. The SNI field shows cloudflare-ech.com]({{site.baseurl}}/images/sni_2.png)


![Screenshot of Wireshark. It shows the encrypted_client_hello extension]({{site.baseurl}}/images/sni_3.png)

## Conclusion

Combining ECH and encrypted DNS significantly boosts privacy, as it prevents third parties (such as ISPs) from observing which domain a user is accessing. While the destination IP address is still visible, many sites today use CDNs with shared IPs, making it difficult to determine the exact website being visited.

Unfortunately, major websites have been slow to adopt ECH. For this reason, I used Cloudflare’s research page as an illustrative example.

If you administrate web servers, consider implementing ECH. If not, help spread the word to raise awareness about ECH and its benefits for privacy for all internet users.