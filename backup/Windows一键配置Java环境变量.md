脚本内容：

```shell
@echo off
chcp 65001
setlocal enabledelayedexpansion

REM 提示用户输入JDK路径
:input_jdk_path
set /p jdk_path=请输入JDK的安装路径（例如：C:\Program Files\Java\jdk1.8.0_221）：

REM 检查路径是否存在
if not exist "%jdk_path%" (
    echo JDK路径无效或不存在，请检查路径后重试。
    pause
    goto input_jdk_path
)

REM 设置JAVA_HOME
setx JAVA_HOME "%jdk_path%"
echo JAVA_HOME已设置为：%jdk_path%

REM 设置CLASSPATH
setx CLASSPATH ".;%%JAVA_HOME%%\lib\dt.jar;%%JAVA_HOME%%\lib\tools.jar;"
echo CLASSPATH已设置。

REM 获取当前Path变量
for /f "skip=2 tokens=3*" %%A in ('reg query "HKCU\Environment" /v "Path" 2^>nul') do (
    set "current_path=%%A %%B"
)

REM 添加新的Path条目
set "new_path=%%JAVA_HOME%%\bin;%%JAVA_HOME%%\jre\bin;%current_path%"

REM 设置新的Path变量
setx Path "%new_path%"
echo Path变量已更新。

echo 环境变量已配置完成。
pause
````
复制脚本内容，在windows新建文件，将内容粘贴进去，将文件后缀改为.bat，双击运行
![image](https://github.com/user-attachments/assets/1443d11f-f358-4b5c-8502-e83d6b3be53a)
![image](https://github.com/user-attachments/assets/22fa7948-b233-443f-81ef-d88c4f689e2d)

