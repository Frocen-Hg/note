当开始菜单中发现很多应用（尤其是的照片、日历、计算器等功能软件）变成了以"ms-resource"开头的形式，甚至有一些应用显示为损坏状态。可以使用，但无法正常显示名称。

根据网络，这种情况大多是由于系统升级、系统还原或使用第三方工具卸载APP所导致的。那么，该如何解决这个问题呢？让我们一起来看看解决方案。

### 解决方法：修复系统

首先，按下"Windows徽标键+X"组合键，启动"Windows PowerShell(管理员)"，然后依次执行以下4条命令：

```powershell
Dism /Online /Cleanup-Image /ScanHealth
Dism /Online /Cleanup-Image /CheckHealth
DISM /Online /Cleanup-Image /RestoreHealth
sfc /SCANNOW
```

> 以及Restore Health 命令进度卡住是正常情况，可能会卡在20% 40% 62%等位置

#### 特殊情况

> Restore Health
>
> 错误: 0x800f081f
> 找不到源文件。
>
> 1：通过ISO文件加载虚拟光驱，配置虚拟盘符
> DISM /Online /Cleanup-Image /RestoreHealth /Source:D:\sources\install.esd /LimitAccess
>
> 2：重置 Windows 更新组件
>
> ```powershell
> net stop wuauserv
> net stop cryptSvc
> net stop bits
> net stop msiserver
> ren C:\Windows\SoftwareDistribution SoftwareDistribution.old
> ren C:\Windows\System32\catroot2 catroot2.old
> net start wuauserv
> net start cryptSvc
> net start bits
> net start msiserver
> ```
>
> 另注：
> 按 Win + R 打开“运行”窗口
> 输入 winver可以查看Windows版本号

命令执行完毕后，重新启动设备，然后在PowerShell中继续执行以下3条命令：

```powershell
taskkill /f /im explorer.exe
Get-AppXPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml"}
start explorer
```

再次重启设备，稍等片刻，查看开始菜单中的"ms-resource:"是否消失，恢复原来的名称。