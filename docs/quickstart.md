# Quickstart

1. 安装最新版本：

```bash
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh
```

```sh
wget -O - https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh
```

2. 查看服务状态：

```bash
systemctl status vohive
```

```sh
/etc/init.d/vohive status
```

3. 查看日志：

```bash
journalctl -u vohive -f
```

```sh
logread -f
```

4. Docker Hub 多架构镜像：

```sh
docker pull skyhotspur/vohive:1.5.5
```
