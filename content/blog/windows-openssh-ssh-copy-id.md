---
title: ssh-copy-id 在 Windows OpenSSH 上不生效的原因与解决
date: 2026-03-15T14:55:00+08:00
draft: false
tags:
  - ssh
  - windows
  - openssh
  - 运维
---

在 Windows 服务器上配置 SSH 免密时，`ssh-copy-id` 经常看起来“成功了”，但登录仍然会要求密码。

常见原因是 Windows OpenSSH 的授权逻辑和 Linux 默认行为不同：

1. 管理员账户默认读取的是 `C:\ProgramData\ssh\administrators_authorized_keys`。
2. `sshd_config` 中的 `Match Group administrators` 会覆盖默认用户目录路径。
3. `ssh-copy-id` 默认写入的是 `~/.ssh/authorized_keys`，因此可能写错位置。

## 解决办法

手动将公钥写入：

```text
C:\ProgramData\ssh\administrators_authorized_keys
```

并修正该文件权限（确保 OpenSSH 可读取且权限符合要求）。

完成后即可实现 Windows 服务器的 SSH 免密登录。
