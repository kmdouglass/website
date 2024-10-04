<!--
.. title: GitHub CLI Authorization with a Fine-grained Access Token
.. slug: github-cli-authorization-with-a-fine-grained-access-token
.. date: 2024-10-04 14:18:48 UTC+02:00
.. tags: github
.. category: 
.. link: 
.. description: 
.. type: text
-->

It is a good idea to use fine-grained access tokens for shared PCs in the lab that require access to private GitHub repos so that you can restrict the scope of their use to specific repositories and not use your own personal SSH keys on the shared machines. I am experimenting with the GitHub command line tool `gh` to authenticate with GitHub using fine-grained access tokens and make common remote operations on repos easier.

Today I encountered a subtle problem in the `gh` authentication process. If you set the protocol to `ssh` during login, then you will not have access to the repos that you granted permissions to in the fine-grained access token. This can lead to a lot of head scratching because it's not at all clear which permissions map to which git operations. In other words, what you think is a specific permissions error with the token is actually an authentication error.

To avoid the problem, be sure to specify `https` and not `ssh` as the protocol during authentication:

```console
 echo "$ACCESS_TOKEN" | gh auth login -p https --with-token
```
