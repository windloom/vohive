# Upgrade

指定版本升级：

```bash
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --version v1.5.5
```

```sh
wget -O - https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --version v1.5.5
```

默认重复执行安装脚本即为升级语义，脚本会备份旧二进制到 `/opt/vohive/bin/vohive.bak`。
