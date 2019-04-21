---
layout: post
title:  "Git - More Secure Commit"
date: 2019-04-21
tags: [git]
excerpt_separator: <!--more-->
---
Hi, so I was exploring about [git signing][git-signing]{:target="_blank"} and it's benefit.

If you have used [git][git-scm]{:target="_blank"}, you must have known that it is a distributed version control system. Every copy of a git repo (including local one) contains the whole information of the repository.

Git service provider such as [Github][github-url]{:target="_blank"}, [Gitlab][gitlab-url]{:target="_blank"} and [Bitbucket][bitbucket-url]{:target="_blank"} provides authentication by using each account credentials. Each has the capability to verify **who** pushed the commit to remote endpoint. This feature isn't part of the [git][git-scm]{:target="_blank"} feature. If we try to check locally, we can't know **who** pushed the commit.
<!--more-->

There are basic information provided in git commit to **try** to identify **who** is doing the commit.
But the information can't be trusted and verified because it can be set by anyone using any value.

For example, with these command, I can set the commit information using **John Doe**'s name and email:
```
git config user.name John Doe
git config user.email john.doe@dronjax.com
```

So, how do we identify **who** actually does the **commit**?
That's what [git signing][git-signing]{:target="_blank"} feature is for.

One way to sign git commits is to use [GPG][gpg-url]{:target="_blank"}, it's an implementation of [OpenPGP][openpgp-url]{:target="_blank"}.

Note: It may look like [GPG][gpg-url]{:target="_blank"} and [OpenPGP][openpgp-url]{:target="_blank"} spelling is similar, but they are different. One is implementation while the other is protocol.

### Setup and Key Generation

First, you need to install [GPG binary][gpg-download-url]{:target="_blank"} to your local machine.

After that, you need to generate a new key by running this command:
```
gpg --generate-key
```

You will be prompted for your informations (Name and Email). For starter, you can fill your **primary email**. We will get to the part where you can add **other emails**.

You will also be prompted to enter passphrase. It is best that you fill it up to secure your key in case it is stolen.

After that, there will be message like these:
```
gpg: key JOHNDOEKEYID marked as ultimately trusted
```

Note that **JOHNDOEKEYID** is the id of the GPG key.

### Associate more emails

If you don't need to add more emails (example: company email), you can skip to [Enable Git Signing](#enable-git-signing).

Nowadays, it's normal that people have multiple emails. Most people probably have 2 or 3 of them.
One of them is bound to be company email which is assigned to them.

Let's assume you use github for hosting company repository. There is a chance that you use your personal account but link the company email. When you want create a commit, you may need to use company email to mark the commit.

For the case above, you don't need to create a separate GPG key to sign the git commit.
One GPG key can be associated with more than one email.

Now, the part where we associate new email starts with:
```
gpg --edit-key JOHNDOEKEYID
```

An interactive GPG shell will show up, you can list the command available by using **help**.
In this post, we just gonna associate a new email by:
```
gpg> adduid
```

Similar to the early part, you need to specify name and email (you can add comment too).
After you enter the information, you will be prompted for key password.

After adding the user, don't forget to run:
```
gpg> save
```

If you don't run the command above, any changes won't be persisted in the GPG key.

### Enable Git Signing

Now, we get to the main part to sign the git commit.
For signing the git commit, we just need to change the git config by running the following command:
```
git config user.signingkey JOHNDOEKEYID
```

After setting up the key to sign in git, you can just sign your commit everytime by adding **-S** parameter. Example:
```
git commit -S -m "Some wonderful message."
```

Or alternatively, you can ensure that every commit always signed by running:
```
git config commit.gpgSign true
```

### Verifying Git Signature

Now now, signing git commit is nice and all, but how do we verify the signed commit.

You can see the signature of the commit in git log by running:
```
git log --show-signature
```

You will need to know the public key of the person to fully verify the commit.

Alternatively, some git service provider provide a way to upload the public key signature.
For example, in [github][github-url]{:target="_blank"} the keys can be added from this [link][github-keys-url]{:target="_blank"}.

To show the public GPG key, you can run the following command:
```
gpg --export --armor JOHNDOEKEYID
```

The result of the command above, can be uploaded to git service provider or shared between coworkers for verification.

If you are using github, then you can see nice sign of verified like the following screenshot:

![Github Verification](/assets/images/github-verification.png)

Or you can also see it yourself at [this link][dronjax-gh-pages-commits-url]{:target="_blank"}.

### Summary

By default, if we use [git][git-scm]{:target="_blank"} basic feature, it is possible for someone to impersonate **John Doe** by changing git author name and email.

Now, since **John Doe** enable git signing on all his commits and share it with his colleagues, all his colleagues can verify his commits. 

**John Doe** has successfully protected himself from impersonation from anyone.

You should start protecting yourself too.

[git-scm]: https://git-scm.com
[github-url]: https://github.com
[bitbucket-url]: https://bitbucket.org
[gitlab-url]: https://gitlab.com
[git-signing]: https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work
[gpg-url]: https://gnupg.org/
[openpgp-url]: https://www.openpgp.org/
[gpg-download-url]: https://gnupg.org/download/index.html
[github-keys-url]: https://github.com/settings/keys
[dronjax-gh-pages-commits-url]: https://github.com/dronjax/dronjax.github.io/commits/master
