---
published: true
description: Discover how to set up and manage an httpd container using Podman on Debian without requiring root privileges. This post provides step-by-step instructions on running containers in a rootless context, enhancing your system's security and leveraging systemd for persistent container management.
tags:
  - linux
  - bash
  - security
title: How To Deploy a Rootless httpd Container with Podman
header:
 image: /images/header/container.webp
 teaser: /images/header/container.webp
---

In this blog post, we'll explore how to set up and run an httpd server (Apache HTTP Server) using a containerized environment with Podman. A key aspect of this setup is running the container in a rootless context, which significantly reduces the risk of system-wide security vulnerabilites. By eliminating the need for root privileges, this method minimizes the potential impact of exploits and enhances the overall security posture of our server. 

For this demonstration, I am using a server with Debian 12. 
If you want to try this setup, feel free to use any Linux distribution that suits your needs.

## Dependencies 

I installed some dependencies beforehand. This was of course the **Podman** package. Additionally, I installed the **uidmap** and **slirp4netns** packages, which are dependencies for running containers rootlessly.
Another essential package is **policykit-1**, necessary to enabling "linger" on an unprivileged user. This feature keeps a user manager running after logouts, providing persistence for our container.  
I also created the user containersrv and then connected to the server via SSH using this newly created user.

## Running a httpd container with Podman

I am using [httpd image](https://hub.docker.com/_/httpd) from the Docker repository.
Let's create a directory in our home directory to use as a persistent volume for the httpd container.

```bash
mkdir ~/content
```

Then we can pull the container image and run the container with the following command. The DocumentRoot within the httpd container is `/usr/local/apache2/htdocs/`. We could change this in the httpd.conf, but to keep it simple, we'll leave it as is for this demonstration.

```bash
podman run -d -p 8080:80 --name apache  -v /home/containersrv/content:/usr/local/apache2/htdocs/ docker.io/library/httpd
```

The Podman command does the following things:

1. Pulls the httpd container image from the Docker repository and runs the container named apache.
2. Maps port 80 within the container to port 8080 on our host OS. In a rootless context, this mapping is necessary because regular users cannot bind to port numbers below 1024.
3. Maps the DocumentRoot path in the container `/usr/local/apache2/htdocs/` to `/home/containersrv/content`

Let's put a little index.html file in our content directory and then view it in our browser by navigating to http://$ip:8080

```bash
echo "hello world. containers are awesome." > content/index.html
```

![Screenshot of the website in the browser]({{site.baseurl}}/images/podman01.png)

It worked; the content we just created is now visible.

## Persist the container as a systemd service

If you reboot the server now, the container will exit and not be running anymore :(.  
However, we want to serve our awesome little website on a regular basis.
Luckily, Podman has a handy feature that allows us to generate a systemd unit file for our container. 

First, let's create the directory structure in our home directory:

```bash
mkdir -p ~/.config/systemd/user/
```

Then, we can change to the newly created directory and create the service unit file with this command:

```bash
cd ~/.config/systemd/user/
podman generate systemd --name apache --new --files
```

Let's review the service file.
We can see that is uses `podman run` as ExecStart action with all the options we specified before.
When the service is stopped it first uses `podman stop` to stop the container and after that the container is completely removed with `podman rm`. This removal is efficient in terms of system resources and is unproblematic because we have a persistent volume for our index.html.

```ini
containersrv@containerlab:~/.config/systemd/user$ cat container-apache.service 
# container-apache.service
# autogenerated by Podman 4.3.1
# Sat Apr 27 08:01:21 UTC 2024

[Unit]
Description=Podman container-apache.service
Documentation=man:podman-generate-systemd(1)
Wants=network-online.target
After=network-online.target
RequiresMountsFor=%t/containers

[Service]
Environment=PODMAN_SYSTEMD_UNIT=%n
Restart=on-failure
TimeoutStopSec=70
ExecStartPre=/bin/rm \
	-f %t/%n.ctr-id
ExecStart=/usr/bin/podman run \
	--cidfile=%t/%n.ctr-id \
	--cgroups=no-conmon \
	--rm \
	--sdnotify=conmon \
	--replace \
	-d \
	-p 8080:80 \
	--name apache \
	-v /home/containersrv/content:/usr/local/apache2/htdocs/ docker.io/library/httpd
ExecStop=/usr/bin/podman stop \
	--ignore -t 10 \
	--cidfile=%t/%n.ctr-id
ExecStopPost=/usr/bin/podman rm \
	-f \
	--ignore -t 10 \
	--cidfile=%t/%n.ctr-id
Type=notify
NotifyAccess=all

[Install]
WantedBy=default.target
```

Now that we have our service unit file we still need to enable and start the service. 
However, before we are doing this we will stop and remove the container first.

```bash
podman stop apache
podman rm apache
```

Then we can enable and start our service. We are using `systemctl --user` because this allows us to run the service within our user context.

```
systemctl --user enable --now container-apache.service
```

To verify that our service and the container is running:

```bash
systemctl --user status container-apache
podman ps
```
![Screenshot of the systemctl status output]({{site.baseurl}}/images/podman02.png)

![Screenshot of the podman ps output]({{site.baseurl}}/images/podman03.png)

Amazing, the service is active and running and the container is running as well. 

As a final step, let's enable lingering to ensure that the user’s manager persists after logouts, allowing the httpd container to continue running in the user context.

```bash
loginctl enable-linger
```

With lingering enabled, the httpd container will now run persistently under the user’s context.