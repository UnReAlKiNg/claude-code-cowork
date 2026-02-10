# claude-code-cowork
使用CLIProxyAPI为claude code提供第三方模型支持

## 安装Node.js
进入 <https://nodejs.org/en/download/> ，下载对应操作系统的node.js。

## 安装git
进入 <https://git-scm.com/install/windows> , 下载对应操作系统的git。

## 安装Claude Code
在windows平台，打开powershell，输入
```powershell
irm https://claude.ai/install.ps1 | iex
```
安装claude code。

安装好后，在`C:\Users\<用户名>\`下有一个 `.claude.json`文件，在里面输入`"hasCompletedOnboarding": true`（用于跳过claude登陆）。

## 安装docker
参考 <https://zhuanlan.zhihu.com/p/24362174734> ，安装docker。

参考 <https://blog.csdn.net/YYDS_54/article/details/148743877> 变更docker镜象位置（避免过多占用c盘）。

## 安装CLIProxyAPI
创建一个文件夹，使用`git clone https://github.com/router-for-me/CLIProxyAPI.git`下载CLIProxyAPI。

下载完后，复制一份`config.example.json`并命名为`config.json`。修改如下参数：  
allow-remote: true  
secert-key: (登陆密码)  

设置好后，在CLIProxyAPI文件夹，依次运行
```powershell
docker compose pull
docker compose up -d
```
来启动服务。

在浏览器输入 <http://localhost:8317/management.html#>，进入管界面，输入刚刚的secret-key登陆。

在【配置面板】中，设置【API密钥列表】。这里可以自己起名字，用于claude code连接。

点击【AI提供商】，选择【OpenAI兼容提供商】，输入以下信息：  
提供商名称: NVIDIA  
Base URL: https://integrate.api.nvidia.com/v1  
模型列表（可以从/models获取，但前提需要先在nvidia-nim中获取对应模型的api-key）  
API密钥: 输入在nvidia-nim中获取的对应模型的api-key  
点击【联通性测试】，验证是否可用。确认没问题后点保存。

回到`config.json`文件，在最底下输入：  
```yaml
payload:
  override:
    - models:
      - name: (你的模型名字）
        protocal: openai(或gemini/claude/codex...)
      params:
        reasoning_effort: high
```
并保存。

## 配置.claude/settings.json文件
进入`C:\Users\<用户名>\.claude\settings.json`，输入以下信息：  
```json
{
  "env":{
    "ANTHROPIC_BASE_URL": "http://localhost:8317",
    "ANTHROPIC_AUTH_TOKEN": "CLIProxyAPI中设置的api密钥",
    "ANTHROPIC_MODEL": "nim中的模型名称",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "CLAUDE_CODE_EFFORT_LEVEL": "high"
  },
  "effortLevel": "high"
}
```
并保存。

在claude code工作区，打开cmd，键入`claude`即可运行。

