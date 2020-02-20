---
layout: post
title: "使用Git登录GitHub报错解决：git@github.com: Permission denied (publickey)."
categories: [git, 行走的问题解决机]
description: "使用Git登录GitHub报错解决：git@github.com: Permission denied (publickey)."
keywords: git
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

我把私钥拷贝到新的机器上，使用ssh登录GitHub，报错：

```sh
-> % ssh -T git@github.com  
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0777 for '/home/vncviewer/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/home/vncviewer/.ssh/id_rsa": bad permissions
git@github.com: Permission denied (publickey).
```
里面提到2个问题，一个是权限，需要把权限设置为`600`；另一个是` bad permissions git@github.com: Permission denied (publickey).`

```sh
-> % chmod 600 ~/.ssh/id_rsa
-> % ssh -T git@github.com  
git@github.com: Permission denied (publickey).
```
还是报错：`git@github.com: Permission denied (publickey).`

GitHub官方有提供解决方案：[Error: Permission denied (publickey) - GitHub Help](https://help.github.com/en/github/authenticating-to-github/error-permission-denied-publickey)

仔细查看ssh的过程：
```sh
-> % ssh -vT git@github.com
OpenSSH_7.6p1 Ubuntu-4ubuntu0.3, OpenSSL 1.0.2n  7 Dec 2017
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to github.com [192.30.253.113] port 22.
debug1: Connection established.
debug1: identity file /home/ubuntu/.ssh/id_rsa type 0
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_rsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_dsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_dsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_ecdsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_ecdsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_ed25519 type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/ubuntu/.ssh/id_ed25519-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.3
debug1: Remote protocol version 2.0, remote software version babeld-17f81433
debug1: no match: babeld-17f81433
debug1: Authenticating to github.com:22 as 'git'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: rsa-sha2-512
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: ssh-rsa SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /home/ubuntu/.ssh/known_hosts:3
Warning: Permanently added the RSA host key for IP address '192.30.253.113' to the list of known hosts.
debug1: rekey after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey after 134217728 blocks
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-rsa,rsa-sha2-512,rsa-sha2-256,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: RSA SHA256:qGvRqGktRexgrlYxy67ZNlfPDUUD31l1loOvzI+7Fmk /home/ubuntu/.ssh/id_rsa
debug1: Authentications that can continue: publickey
debug1: Trying private key: /home/ubuntu/.ssh/id_dsa
debug1: Trying private key: /home/ubuntu/.ssh/id_ecdsa
debug1: Trying private key: /home/ubuntu/.ssh/id_ed25519
debug1: No more authentication methods to try.
git@github.com: Permission denied (publickey).
```
看到里面的报错是`key_load_public: No such file or directory`。

然后我接着查找问题，发现私钥拷贝错误，把另一个私钥拷贝过来了。。。。。。。

私钥正确后即可成功连接：

```sh
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-rsa,rsa-sha2-512,rsa-sha2-256,ssh-dss>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: RSA SHA256:14HZgHvg4z5qnFwlcrUeZ7AzxOpp+cka9oEMPrr04YQ /home/ubuntu/.ssh/id_rsa
debug1: Server accepts key: pkalg ssh-rsa blen 535
debug1: Authentication succeeded (publickey).
Authenticated to github.com ([140.82.113.3]:22).
debug1: channel 0: new [client-session]
debug1: Entering interactive session.
debug1: pledge: network
debug1: Sending environment.
debug1: Sending env LANG = en_US.utf8
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
Hi zhang0peter! You've successfully authenticated, but GitHub does not provide shell access.
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 3412, received 2484 bytes, in 0.4 seconds
Bytes per second: sent 8131.7, received 5920.1
debug1: Exit status 1
```
