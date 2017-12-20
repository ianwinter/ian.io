---
layout: post
title: Securing SSH
tags: security hardening ssh
---

I came across a tool on GitHub today thanks to a colleague. [SSH-Audit](https://github.com/arthepsy/ssh-audit) is a tool for auditing an ssh server.

You can run it right from your machine against any host that you're allowed to:

```
$ ./ssh-audit.py [-1246pbnvl] <host>
```

You can deploy the changes to both your servers, but, you can also add them to your own client `.ssh/config` to ensure what you're connecting to is as secure as it can be based on your configuration. For defense-in-depth, doing both is ideal. You'll need to run ssh-audit after upgrades to `openssl`, `openssh` in case new issues come up.

During the process of reviewing the results and making changes I came across [this gist](https://gist.github.com/terrywang/a4239989b79d34f4160b) which has both sample configuration and details how github.com, sometimes, requires a different set of key exchange algorithms. Worth bearing in mind when you are unable to pull any repositories!.

# Results

After the changes had been made, I've now got a server with no warnings and no fails.

``` shell
$ ./ssh-audit.py <host>
# general
(gen) banner: SSH-2.0-OpenSSH_7.4
(gen) software: OpenSSH 7.4
(gen) compatibility: OpenSSH 7.3+, Dropbear SSH 2016.73+
(gen) compression: enabled (zlib@openssh.com)

# key exchange algorithms
(kex) curve25519-sha256@libssh.org   -- [info] available since OpenSSH 6.5, Dropbear SSH 2013.62
(kex) diffie-hellman-group16-sha512  -- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73
(kex) diffie-hellman-group18-sha512  -- [info] available since OpenSSH 7.3
(kex) diffie-hellman-group14-sha256  -- [info] available since OpenSSH 7.3, Dropbear SSH 2016.73

# host-key algorithms
(key) ssh-rsa                        -- [info] available since OpenSSH 2.5.0, Dropbear SSH 0.28
(key) rsa-sha2-512                   -- [info] available since OpenSSH 7.2
(key) rsa-sha2-256                   -- [info] available since OpenSSH 7.2
(key) ssh-ed25519                    -- [info] available since OpenSSH 6.5

# encryption algorithms (ciphers)
(enc) chacha20-poly1305@openssh.com  -- [info] available since OpenSSH 6.5
                                     `- [info] default cipher since OpenSSH 6.9.
(enc) aes128-ctr                     -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52
(enc) aes192-ctr                     -- [info] available since OpenSSH 3.7
(enc) aes256-ctr                     -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52
(enc) aes128-gcm@openssh.com         -- [info] available since OpenSSH 6.2
(enc) aes256-gcm@openssh.com         -- [info] available since OpenSSH 6.2

# message authentication code algorithms
(mac) umac-128-etm@openssh.com       -- [info] available since OpenSSH 6.2
(mac) hmac-sha2-256-etm@openssh.com  -- [info] available since OpenSSH 6.2
(mac) hmac-sha2-512-etm@openssh.com  -- [info] available since OpenSSH 6.2
```

# Notes

One issue I encountered was with the `ssh-ed25519` host-key algorithm. Even though the changes needed to use it were there, it wouldn't show as available. For some reason (still unknown) in the `sshd_config` on the particular server the host key was missing. Adding this line `HostKey /etc/ssh/ssh_host_ed25519_key` along side the other `HostKey` entries resolved this.

You're almost certainly going to need to remove `.ssh/known_hosts` as each host you connect to will have it's key stored differently after these changes. Whilst a pain, removing the file was faster in the long run for me.

Don't forget to consider if there are any hosts, for any reason, you need a specific key/cipher/mac set for.

# References and Resources
- [SSH Hardening via ansible](https://github.com/dev-sec/ansible-ssh-hardening)
- [SSH-Audit Tool](https://github.com/arthepsy/ssh-audit)
- [Sample Config & GitHub Variant](https://gist.github.com/terrywang/a4239989b79d34f4160b)

