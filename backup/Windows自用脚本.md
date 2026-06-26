### 1.Navicat试用脚本：

```
curl -O https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/navicat-reset.cmd
```

### 2.Windows解除本地回环限制：
在Powershell中输入：

```
 Get-ChildItem -Path Registry::"HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings\" -name | ForEach-Object {CheckNetIsolation.exe LoopbackExempt -a -p="$_"} 
```

### 3.Winget安装指令：

```
$progressPreference = 'silentlyContinue'
Write-Host "Installing WinGet PowerShell module from PSGallery..."
Install-PackageProvider -Name NuGet -Force | Out-Null
Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery | Out-Null
Write-Host "Using Repair-WinGetPackageManager cmdlet to bootstrap WinGet..."
Repair-WinGetPackageManager
Write-Host "Done."
```

### 4.常用软件安装：

日常软件：

```
winget install --id Yuanli.uTools -e
winget install --id Tencent.WeChat.Universal -e
winget install --id Tencent.WeType -e
winget install --id 7zip.7zip -e
winget install --id Git.Git -e
winget install --id Google.Chrome -e
winget install --id Termius.Termius -e
winget install Notepad--
winget install --id appmakes.Typora -e
winget install --id voidtools.Everything -e
winget install --id JetBrains.Toolbox -e
winget install --id Telegram.TelegramDesktop -e
winget install --id Microsoft.VisualStudioCode -e
winget install --id GeekUninstaller.GeekUninstaller -e
```

Windows 开发 / AI 时代工具链：

```
# 终端、Shell、启动器
winget install --id Microsoft.WindowsTerminal -e
winget install --id Microsoft.PowerShell -e
winget install --id Microsoft.PowerToys -e

# Linux-like 命令行体验
winget install --id Microsoft.WSL -e
winget install --id Microsoft.Coreutils -e
winget install --id Microsoft.Edit -e

# AI / Agent / 本地模型相关
winget install --id Microsoft.AIShell -e
winget install --id Microsoft.FoundryLocal -e
winget install --id Microsoft.IntelligentTerminal -e

# 开发常用补充
winget install --id GitHub.cli -e
winget install --id Microsoft.Sysinternals -e
winget install --id Docker.DockerDesktop -e
winget install --id OpenJS.NodeJS.LTS -e
winget install --id Python.Python.3.13 -e
winget install --id Microsoft.VisualStudio.2022.Community -e
```

WSL 初始化：

```
wsl --install
wsl --update
wsl --set-default-version 2
```
