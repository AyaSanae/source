---
title: 解决git clone 完成后提示 error:RPC failed;curl 56 GnuTLS recv error (-9)
date: 2021-02-10 13:21
categories:
    Fix Error
tags:
    Git
description: error:RPC failed;curl 56 GnuTLS recv error (-9)
---

错误提示:
```
remote: Enumerating objects: 9817, done.
error: RPC failed; curl 56 GnuTLS recv error (-9): A TLS packet with unexpected length was received.
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed
```

解决方法:
```
apt-get install gnutls-bin
git config --global http.sslVerify false
git config --global http.postBuffer 1048576000
```

转自:https://blog.csdn.net/tmaccs/article/details/101289284