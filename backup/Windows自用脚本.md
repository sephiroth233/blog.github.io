mihomo内核辅助脚本：
```
curl -O https://raw.githubusercontent.com/sephiroth233/bat-reo/main/blash.cmd
```
JDK环境变量配置脚本：
```
curl -O https://raw.githubusercontent.com/sephiroth233/bat-reo/main/jdk.cmd
```
navicat试用脚本：
```
curl -O https://raw.githubusercontent.com/sephiroth233/bat-reo/main/navicat-reset.cmd
```
Windows解除本地回环限制：
在Powershell中输入：
```
 Get-ChildItem -Path Registry::"HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings\" -name | ForEach-Object {CheckNetIsolation.exe LoopbackExempt -a -p="$_"} 
```
Winget安装指令：
```
$progressPreference = 'silentlyContinue'
Write-Host "Installing WinGet PowerShell module from PSGallery..."
Install-PackageProvider -Name NuGet -Force | Out-Null
Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery | Out-Null
Write-Host "Using Repair-WinGetPackageManager cmdlet to bootstrap WinGet..."
Repair-WinGetPackageManager
Write-Host "Done."
```
软件安装：
```
 winget install --id Yuanli.uTools
 winget install --id Tencent.WeChat.Universal
 winget install --id 7zip.7zip
 winget install --id Git.Git
 winget install --id Google.Chrome
 winget install --id Termius.Termius
 winget install --id Obsidian.Obsidian
 winget install --id Kopia.KopiaUI
 winget install Notepad--
 winget install --id appmakes.Typora
 winget install --id voidtools.Everything
 winget install --id JetBrains.Toolbox
 winget install --id Telegram.TelegramDesktop
 winget install emby 
 winget install --id kangfenmao.CherryStudio
 winget install --id Notion.Notion
 winget install --id Microsoft.VisualStudioCode
 winget install --id GeekUninstaller.GeekUninstaller
```