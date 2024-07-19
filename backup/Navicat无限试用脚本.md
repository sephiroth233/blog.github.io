```shell
@echo off
echo Delete HKEY_CURRENT_USER\Software\PremiumSoft\NavicatPremium\Update
reg delete HKEY_CURRENT_USER\Software\PremiumSoft\NavicatPremium\Update /f
echo Delete HKEY_CURRENT_USER\Software\PremiumSoft\NavicatPremium\Registration[version and language]
for /f %%i in ('"REG QUERY "HKEY_CURRENT_USER\Software\PremiumSoft\NavicatPremium" /s | findstr /L Registration"') do (
    reg delete %%i /va /f
)

echo Delete Info and ShellFolder under HKEY_CURRENT_USER\Software\Classes\CLSID
for /f "tokens=*" %%a in ('reg query "HKEY_CURRENT_USER\Software\Classes\CLSID"') do (
  for /f "tokens=*" %%l in ('reg query "%%a" /f "Info" /s /e ^| findstr /i "Info"') do (
    echo Delete: %%a
    reg delete %%a /f
  )
  for /f "tokens=*" %%l in ('reg query "%%a" /f "ShellFolder" /s /e ^| findstr /i "ShellFolder"') do (
    echo Delete: %%a
    reg delete %%a /f
  )
)
```

新建一个txt文件，名字随意，以reset.txt为例：

复制上面的脚本代码，将代码粘贴到reset.txt文件中。

重命名文件将其更改为reset.bat，注意修改的是文件的后缀名。


### 设置定时任务，定时执行脚本:

#### 打开任务计划程序：

按下 Win + R，输入 taskschd.msc，然后按 Enter。

#### 创建基本任务：

在“任务计划程序”窗口的右侧，选择“创建基本任务…”。

输入任务的名称和描述，然后点击“下一步”。

##### 选择触发器：

选择任务的触发时间（如每日、每周、每月等），然后点击“下一步”。

根据你的选择，设置具体的触发时间和日期，然后点击“下一步”。

#### 设置操作：

选择“启动程序”，然后点击“下一步”。

点击“浏览…”按钮，选择你要运行的BAT文件，然后点击“下一步”。

#### 完成任务：

查看你的设置，确认无误后点击“完成”。

