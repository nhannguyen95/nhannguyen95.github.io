---
layout: post
title: "Change Github account (user) at local on MacOS"
date: 2018-07-20 20:48:00 +0700
tags:
  - tech
  - git
comments: true
mathjax: true
content_level: 1
img:
summary: This post shows how to change Github user at local on MacOS.
---

Firstly, you need to set your new Github's username and email. The email is crucial because Github will base on that to identify your account.

{% highlight shell %}
git config --global user.name "username"  # who gets credit for changes
git config --global user.email "your@email.com"
{% endhighlight %}

Secondly, you need to delete the credential of the old Github account. To do this on MacOS, access `Application/Utilities/Keychain Access`, search for `github.com` and delete all related keychains.

Then, in the next time you use `push`, you will be asked to enter your Github's username and password. In case you activated [2FA](https://authy.com/what-is-2fa/) on your Github account, you will need to enter the Github's `Personal Access Token` instead of the password. You can learn how to generate it [here](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

Finally, to avoid git asks you for the credential information again and again, [you can save it to a `credential helper`](https://help.github.com/articles/caching-your-github-password-in-git/).

That's it!
