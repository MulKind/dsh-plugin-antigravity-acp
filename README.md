# dsh-plugin-antigravity-acp

DeepSeek Harness (`dsh`) 原生插件包（Bundle），用于通过 **ACP (Agent Client Protocol)** 接入本机 **Antigravity CLI (`agy acp`)**，并默认注入 **V2RayN 代理透传 (`http://127.0.0.1:10808`)**。

## 1. 在 DSH 会话 UI 中直接切换 Antigravity（配合 `@zaimokuza/dsh-acp-adapter`）

1. 在 DSH 中安装 ACP 适配器插件：
   ```powershell
   # Web Profile:
   npx @deepseek-ai/dsh plugin --profile web add @zaimokuza/dsh-acp-adapter

   # Desktop Profile（需先退出 DSH 桌面端）:
   dsh plugin --profile desktop add @zaimokuza/dsh-acp-adapter
   ```
2. 运行工作区中的自动配置脚本（会自动识别本机 `C:\Users\<用户名>\AppData\Local\agy\bin\agy.exe` 绝对路径并写入 `127.0.0.1:10808` 代理环境变量与 `NO_PROXY` 回环豁免）：
   ```powershell
   powershell -ExecutionPolicy Bypass -File ..\dsh-antigravity-acp\scripts\Configure-DshAntigravityAcp.ps1 -Profile web -Mode both
   ```
3. 启动或重启 DSH，在会话界面的模型选择器中即可直接选择 **Antigravity · ACP**。

## 2. 作为 DSH 原生插件包安装（提供 `subagent_antigravity` 委托工具）

```powershell
npx @deepseek-ai/dsh plugin --profile web add "D:\Antigravity_Projects\Create Plugins\dsh-plugin-antigravity-acp"
```

安装后，DSH 的 `dsh.profile.bundles` 会自动挂载本包的 `cordis.patch.yml`：
- 注册 `@deepseek-ai/dsh-subagent-acp` provider (`providerName: antigravity`)
- 暴露全局委托工具 `subagent_antigravity`
- 自动透传 `HTTP_PROXY` / `HTTPS_PROXY` / `ALL_PROXY` (`http://127.0.0.1:10808`) 与 `NO_PROXY` (`localhost,127.0.0.1,::1,.local`)
