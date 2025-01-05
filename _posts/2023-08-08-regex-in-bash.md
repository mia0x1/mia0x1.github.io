---
published: true
description: Regular expressions (RegEx) are powerful pattern matching tools in Bash. Discover how they are used in if queries to match patterns and solve problems such as validating user input and extracting text elements.
  - linux
  - opensource
  - macos
header:
  image: /images/header/regex.webp
  teaser: /images/header/regex.webp
  image_description: A developer sits in front of a laptop and writes code. In the background, you can see the night sky from the window.
title: RegEx in if statements - Effective pattern recognition in Bash
---

Regular expressions, often abbreviated as RegEx, are specialized strings used for pattern matching. When combined with Bash, they form an extremely powerful tool. Knowledge of this is particularly useful for administrators working with Bash in Linux or other Unix-like systems to solve problems efficiently.

![Bash Logo]({{site.baseurl}}/images/bash_logo.png)

Regular expressions are especially helpful in if statements. For instance, they can be used to validate user input or extract and process specific elements from a text.

## Syntax

To use RegEx in an if statement, the =~ operator is required.
This allows a string to be matched against a regular expression.

```bash
if [[ "$input" =~ regex ]]; then
  # Command to execute if $input matches the regular expression.
else
  # Command to execute if $input does not match the regular expression.
fi
```
This if statement checks the content of the variable $input against a regular expression (not further defined here).

Here are some concrete examples for illustration:

## Checking if $input is a number

In the following example, the if statement checks whether the content of $input is a whole number.

```bash
if [[ "$input" =~ ^[0-9]+$ ]]; then
  # Command to execute if $input is a whole number.
else
  # Command to execute if $input is not a whole number.
fi
```

## Checking if $input consists of letters

With this regular expression, you can check if the content of $input consists exclusively of uppercase and lowercase letters.

```bash
if [[ "$input" =~ ^[A-Za-z]+$ ]]; then
  # Command to execute if $input consists of uppercase and lowercase letters.
else
  # Command to execute if $input does not consist of uppercase and lowercase letters.
fi
```

These two examples represent relatively simple applications of regular expressions. However, they can quickly become more complex. For creating your own regular expressions, I find the website [regex101.com](https://regex101.com/) particularly helpful. In addition to comprehensive documentation, it also provides the ability to test and verify your own regular expressions with custom inputs.
Have you ever used RegEx in if statements before? If so, feel free to share your experiences with me. :)

