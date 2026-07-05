# Rollback

1. 直接安装指定版本：

```bash
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --version v1.5.5
```

```sh
wget -O - https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --version v1.5.5
```

2. 若升级失败且 `.bak` 存在，可手动恢复：

```bash
sudo cp /opt/vohive/bin/vohive.bak /opt/vohive/bin/vohive
sudo systemctl restart vohive
```

```sh
cp /opt/vohive/bin/vohive.bak /opt/vohive/bin/vohive
/etc/init.d/vohive restart
```
