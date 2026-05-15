# cc-connect-fork

基于 [chenhg5/cc-connect](https://github.com/chenhg5/cc-connect) 的 fork，增加了 Discord `allow_bots` 支持。

## 与原版的区别

### `allow_bots` 功能（Discord）

原版 cc-connect 会硬过滤所有 bot 消息（`m.Author.Bot`），导致两个 bot 之间无法通过 Discord 通信。本 fork 新增 `allow_bots` 配置项：

```toml
[[projects.platforms]]
type = "discord"

[projects.platforms.options]
allow_bots = true
```

开启后，其他 bot 发送的消息可以被接收和处理（自己的消息仍然过滤，不会自己回自己）。

**使用场景：** 两个 AI agent（如 OpenClaw + Hermes）通过 Discord 频道互相通信。

## 配置说明

### Windows 配置路径

配置文件位于 `C:\Users\<用户名>\.cc-connect\config.toml`

示例配置见 [config-example/config.toml](config-example/config.toml)

### 关键配置项

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `allow_bots` | 允许接收其他 bot 的消息 | `false` |
| `allow_from` | 允许的用户ID，`*` 为全部 | 空（全部） |
| `guild_id` | Discord 服务器 ID | 可选 |
| `channel_id` | 监听的频道 ID | 可选 |

## 编译

需要 Go 1.24+（Windows 侧编译）：

```bash
go build -tags no_web -o cc-connect.exe ./cmd/cc-connect/
```

> `no_web` tag 跳过 web 前端（需要 node 构建），不影响核心功能。

## 安装

### 方式一：替换 npm 安装的二进制

npm 全局安装的 exe 位于：
```
C:\Users\<用户名>\AppData\Roaming\npm\node_modules\cc-connect\bin\cc-connect.exe
```

先停掉 cc-connect，然后替换：

```bash
# 备份旧的
copy cc-connect.exe cc-connect.exe.bak
# 复制新编译的
copy D:\work\cc-connect\cc-connect.exe <上面的路径>\cc-connect.exe
```

启动方式不变：`cc-connect` 命令。

### 方式二：直接运行 exe

```bash
cc-connect.exe
```

> ⚠️ 注意：如果通过 npm 的 `cc-connect.cmd` 启动，`run.js` 会检查版本号，版本不匹配可能触发自动重装覆盖。直接运行 exe 可绕过此检查。

## 与上游同步

```bash
git fetch upstream
git merge upstream/main
```

## License

同原项目 [MIT License](https://github.com/chenhg5/cc-connect/blob/main/LICENSE)。
