<div align="center">

# MirrorServerReforged

**为 MCDReforged 服务器同步、启动与管理独立镜像服**

[![Python](https://img.shields.io/badge/Python-plugin-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![MCDReforged](https://img.shields.io/badge/MCDReforged-%E2%89%A52.6.0-4C8BF5)](https://mcdreforged.com/)
[![Version](https://img.shields.io/badge/Version-1.0.8--alpha-orange)](mcdreforged.plugin.json)
[![License](https://img.shields.io/badge/License-Apache--2.0-blue)](LICENSE)

[![Stars](https://img.shields.io/github/stars/HY916-cn/MirrorServerReforged?logo=github&label=Stars)](https://github.com/HY916-cn/MirrorServerReforged/stargazers)
[![Forks](https://img.shields.io/github/forks/HY916-cn/MirrorServerReforged?logo=github&label=Forks)](https://github.com/HY916-cn/MirrorServerReforged/network/members)
[![Last Commit](https://img.shields.io/github/last-commit/HY916-cn/MirrorServerReforged?logo=git&logoColor=white)](https://github.com/HY916-cn/MirrorServerReforged/commits/master)
[![Repo Size](https://img.shields.io/github/repo-size/HY916-cn/MirrorServerReforged)](https://github.com/HY916-cn/MirrorServerReforged)
[![Issues](https://img.shields.io/github/issues/HY916-cn/MirrorServerReforged?logo=github&label=Issues)](https://github.com/HY916-cn/MirrorServerReforged/issues)

将主服世界复制到独立目录，并以 MCDReforged、Vanilla 或其他服务端命令启动镜像服；<br/>
可选通过 RCON 安全关闭镜像进程。

</div>

---

本仓库是 [EMUnion/MirrorServerReforged](https://github.com/EMUnion/MirrorServerReforged) 的 fork，插件源自 [MCDR-Mirror-Server](https://github.com/GamerNoTitle/MCDR-Mirror-Server)。

## 功能特性

- **世界同步**：将一个或多个世界目录复制到镜像服，同步时排除 `session.lock`。
- **双模式运行**：支持以 MCDReforged 目录结构初始化镜像服，也可配置 Vanilla、Bukkit 等普通服务端的启动命令。
- **独立进程**：以配置的 `command` 在 `Mirror` 目录中启动镜像服。
- **RCON 停服**：启用 RCON 后，可从主服向镜像服发送 `stop` 命令。
- **热重载**：修改 JSON 配置后可通过指令重新读取，无需重启主服。

## 环境要求

- MCDReforged 2.6.0 或更高版本
- 一个可由命令行启动的 Minecraft 镜像服务端
- 如需使用 `!!msr stop`，镜像服必须启用 RCON

## 安装

克隆仓库，然后使用 MCDReforged 的 [`pack`](https://docs.mcdreforged.com/en/latest/cli/pack.html) 命令生成 `.mcdr` 插件包：

```bash
git clone https://github.com/HY916-cn/MirrorServerReforged.git
cd MirrorServerReforged
mcdreforged pack -i . -o ./dist
```

将 `dist/` 中生成的 `.mcdr` 文件放入主服 MCDReforged 的 `plugins/` 目录，再在控制台执行：

```text
!!MCDR plugin reloadall
```

插件首次加载时会在 `config/` 中生成 `MirrorServerReforged.json`，并创建镜像服所需的 `Mirror/` 目录。

## 配置

```json
{
  "world": ["world"],
  "command": "python3 -m mcdreforged",
  "rcon": {
    "enable": false,
    "host": "localhost",
    "port": 25575,
    "password": "password"
  },
  "source": "./server",
  "target": "./Mirror/server"
}
```

| 字段 | 说明 |
| --- | --- |
| `world` | 需要同步的世界目录列表；Bukkit 类服务端可加入 `world_nether`、`world_the_end` 等目录 |
| `command` | 在镜像目录中执行的启动命令，例如 `python3 -m mcdreforged` 或 `java -Xmx16G -Xms1G -jar server.jar nogui` |
| `rcon.enable` | 是否允许插件通过 RCON 关闭镜像服 |
| `rcon.host` | 镜像服 RCON 地址 |
| `rcon.port` | 镜像服 RCON 端口，需与主服及游戏端口区分 |
| `rcon.password` | 镜像服 RCON 密码 |
| `source` | 主服服务端目录 |
| `target` | 镜像服服务端目录 |

启用 RCON 时，请立即更换默认密码，限制 `config/MirrorServerReforged.json` 的文件权限，并确保 RCON 端口不向公网开放。

## 命令

| 命令 | 作用 |
| --- | --- |
| `!!msr help` | 显示帮助与当前版本 |
| `!!msr sync` | 请求主服保存并同步配置的世界目录 |
| `!!msr reload` | 重新读取插件配置 |
| `!!msr start` | 启动镜像服进程 |
| `!!msr stop` | 通过 RCON 关闭镜像服 |
| `!!msr init` | 初始化 MCDReforged 模式的镜像服目录 |

## License

本项目依照 [Apache License 2.0](LICENSE) 提供。
