---
title: 合并或更新Wlan驱动
subtitle:
date: 2020-06-21 17:20
comments: true
categories:
    - Kernel
tags:
    - Git
excerpt: "无非就是GIT的运用"
---
## 首次合并
```
git remote add qcacld-3.0 https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/qcacld-3.0

git fetch qcacld-3.0 <TAG>

git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD

git read-tree --prefix=drivers/staging/qcacld-3.0 -u FETCH_HEAD

git commit

```

## 更新tag:
```
git fetch qcacld-3.0 <TAG>

git merge -X subtree=drivers/staging/qcacld-3.0 FETCH_HEAD
```

 
## qca-wifi-host-cmn和fw-api也是这样
qcacld-3.0 source: https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/qcacld-3.0
fw-api source: https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/fw-api
qca-wifi-host-cmn source: https://source.codeaurora.org/quic/la/platform/vendor/qcom-opensource/wlan/qca-wifi-host-cmn