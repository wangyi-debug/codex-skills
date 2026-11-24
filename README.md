# Codex Skills Collection

这是一个可直接克隆使用的 Codex Skills 技能包，共包含 26 个技能。

## 下载

```bash
git clone https://github.com/wangyi-debug/codex-skills.git
```

## 安装到 Codex

### Windows PowerShell

```powershell
$target = Join-Path $HOME ".agents\skills"
New-Item -ItemType Directory -Path $target -Force | Out-Null
Get-ChildItem -Path .\codex-skills -Directory | Copy-Item -Destination $target -Recurse -Force
```

### macOS / Linux

```bash
mkdir -p ~/.agents/skills
cp -R codex-skills/* ~/.agents/skills/
```

安装后重新启动 Codex，或新建一个会话，让技能列表重新加载。

## 技能来源与许可证

本仓库用于同步和分发个人 Codex 环境中的技能集合，不表示对其中第三方技能的原创声明。

- 多数 `baoyu-*` 技能来源于 [JimLiu/baoyu-skills](https://github.com/JimLiu/baoyu-skills)。
- `jianying-editor` 来源于 [luoluoluo22/jianying-editor-skill](https://github.com/luoluoluo22/jianying-editor-skill)，其原许可证已保留在对应目录中。
- 其他技能的版权和许可条款以各自目录中的说明及上游项目为准。

公开使用、修改或再分发前，请确认相应上游项目的许可证要求。

## 安全说明

仓库不包含个人 API Key、登录凭据或本机 `.git` 历史。需要外部服务的技能，请在自己的环境变量或本地配置中设置凭据，不要提交到 Git。
