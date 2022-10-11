---
title: Windows-Terminal美化
date: 2022-10-09 10:43:08
categories:
  - shell  
tags:
  - Windows
---

**颜值就是生产力**

<!--more-->
## 美化PowerShell
> 1. 升级 Windows Terminal
> 到 [Microsoft/Terminal](https://github.com/microsoft/terminal/releases) 找最新的的版本下载，双击安装。
> 
> 2. 安装powershell最新版 
> 到 [Microsoft/Powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2)寻找下载方法，我用的是：`winget`
> 
> 3. 安装 Oh My Posh
>    最好以管理员身份运行powershell。
>    ```console
>    winget install JanDeDobbeleer.OhMyPosh -s winget
>    ```
>    这个下载内容包括两个东西:
>    - oh-my-posh.exe-Windows executable
>    这个是基于Windows系统的oh-my-posh的可执行文件, 但是点击运行没有用, 必须要在Powershell中执行。在Powershell输入oh-my-posh可以运行程序，如果报错，查看path环境变量是否包含![](https://i.postimg.cc/15QHCpYd/1.jpg)
如果包含的话，说明该环境变量在当前程序不起作用，重启终端即可。
>    查看oh-my-posh版本
>        ```console
>        oh-my-posh version
>        ```
>    - theme
>        oh-my-posh的主题。
>        查看环境变量是否包含：
>        ![](https://i.postimg.cc/XJfZXTpn/2.jpg)
>
>    [官方文档](https://ohmyposh.dev/docs/installation/windows)给出了oh-my-posh的更新命令, 如果是刚刚下载的那么就不需要更新了。
>    ```console
>     winget upgrade JanDeDobbeleer.OhMyPosh -s winget
>    ```
> 
> 4. 配置主题
> 在对powershell进行个性化配置oh-my-posh的时候，powershell需要运行配置脚本文件$PROFILE，由于win11默认防止执行不信任的脚本文件，导致配置过程无法正常进行。
> 参考[博客](https://blog.csdn.net/ahahayaa/article/details/125470163)
> 执行命令:
>    ```console
>    set-ExecutionPolicy RemoteSigned 
>    ``` 
>    1. 使用以下这行命令进行主题的初始化, 其中jandedobbeleer是主题的名字  
> `oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json"`
>    2. 主题的更换要依托于一个PROFILE脚本文件来进行
基于PROFILE脚本文件涉及到 创建、打开、填充配置语句、执行脚本文件 四条命令
>    - 创建PROFILE文件
>        `New-Item -Path $PROFILE -Type File -Force`
>    - 打开PROFILE文件    
>        `notepad $PROFILE`
>    - 在PROFILE文件中添加以下内容
>        `oh-my-posh init pwsh | Invoke-Expression`
>    - 执行PROFILE脚本文件, 通过以下这条命令   
>        `. $PROFILE` //执行脚本文件命令
> 更换主题，修改`$profile`文件对应主题名即可。
>    ```console
>    oh-my-posh init pwsh --config ~\AppData\Local\Programs\oh-my-posh\themes\atomic.omp.json | Invoke-Expression
>    ```
>    **建议**按照这个流程来，之前我在装了win11的新电脑配置oh-my-posh的时候，与3、4两步配置流程上有点差异(但是同样的配置方法在我win10的电脑上是有效的)，导致配置完成非常怪，显示的主题无法更改且并非默认主题，并且pwsh启动时间长达5s，之后按照3、4流程走一遍还是达到了预期效果。
> 
> 5. 修改字体
> Oh My Posh 的部分主题需要相应的字体支持，否则会出现乱码。
> 安装`Nerd Font`系列字体。[字体下载地址](https://www.nerdfonts.com/font-downloads)
> 我安装的是`Caskaydia Vove Nerd Font`。
> 在shell的配置中选择字体：
> ![](https://i.postimg.cc/sX7BgQCv/1.jpg)
> **顺便说一下**，这里的配色方案就是Windows Terminal配置文件中的scheme，也是可以自己配置的。
> 
> **安装插件可以参考**[安裝新版 Oh My Posh 與插件](https://www.kwchang0831.dev/dev-env/pwsh/oh-my-posh)
>
> 6. 安裝 Scoop
> Scoop 就像Mac 的 Homebrew 一样让可以我们更快速地用指令行安装软件。
> pwsh 输入
>    ```console
>    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
>    Invoke-WebRequest get.scoop.sh | Invoke-Expression
>    ```
>
> 7. 安装插件posh-git
> posh-git 让 Git 的指令可以用 Tab 补全。
> pwsh 输入
>    ```console
>    scoop bucket add extras
>    scoop install posh-git
>    Add-PoshGitToProfile
>    ```
>     在 `$Profile` 档案最后一行新增以下指令：
>    ```console
>    Import-Module posh-git
>    ```
> 
> 8. 安装插件ZLocation
> ZLocation 类似于 autojump 或是 Zsh-z 的插件，
可以用关键字直接跳到想去的资料夹，比使用 `cd` 更快速。
> 用PowerShell 输入以下指令：
>    ```console
>    Install-Module ZLocation -Scope CurrentUser
>    ```
>     输入[A] Yes to All ，全部同意。
>     在 $Profile 档案最后一行新增以下指令，
>    ```console
>    Import-Module ZLocation
>    ```
>    ZLocation 使用方式:
>    - 查看已知的文件夹位置，指令：`z`
>    - 进入包含字符串`string`的文件夹（不是文件夹路径包含字符串），可以用 Tab 来选择结果，指令：`z string`
>    - 回到之前的文件夹，指令：`z -`
>    
> 9. (选用) 安装NeoFetch
> NeoFetch 用来显示电脑配置。
> 
> 如果过去用`Install-Module` 的方式安装`Oh My Posh`，即使用`Install-Module oh-my-posh -Scope CurrentUser`，需要删除旧版本，用命令`Uninstall-Module oh-my-posh -AllVersions`，并且删除`$Profile`里的 `Import-Module oh-my-posh`，按前面的方法重新安装。
> 
> **注意：** 在`$Profile`中添加的内容越多，对应的shell启动时间越慢

### 补充说明
> 查看shell的配置文件的文件地址 
>```console
> $profile
>```

> 在pwsh输入命令可以直接用vsc打开配置文件
>```console
> code $profile
>```

> Windows PowerShell `$profile` 地址
> `~\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`
> ```console
> oh-my-posh --init --shell pwsh --config ~\AppData\Local\Programs\oh-my-posh\themes\craver.omp.json | Invoke-Expression
>```
>
> PowerShell 7 `$profile` 地址
> `~\PowerShell\Microsoft.PowerShell_profile.ps1`
> ```console
> oh-my-posh --init --shell pwsh --config ~\AppData\Local\Programs\oh-my-posh\themes\jandedobbeleer.omp.json | Invoke-Expression
>```

> 让`$profile`配置立即生效
>```console
> . $profile
>```

> 下载的 Oh My Posh 主题在文件夹`~\AppData\Local\Programs\oh-my-posh\themes`，里面有包括`craver.omp.json 、jandedobbeleer.omp.json`在内的许多`.omp.json`文件，修改`$profile`对应位置即可修改主题。
> 预览主题在 [Oh my posh 官网](https://ohmyposh.dev/docs/themes)
> 也可以输入命令查看
>```console
> Get-PoshThemes
>```
> **随便推荐几个主题:**
> ![1](https://i.postimg.cc/xd1Gv0nT/1.jpg)
> ![2](https://i.postimg.cc/8PC1g9pf/2.jpg)
> ![3](https://i.postimg.cc/d1JbmZDZ/3.jpg)
> ![4](https://i.postimg.cc/TY54bJQH/4.jpg)
> ![5](https://i.postimg.cc/fLg8tVdN/5.jpg)
> ![6](https://i.postimg.cc/g2Wbk7dK/6.jpg)
> ![7](https://i.postimg.cc/gcCfN7wP/7.jpg)
> ![8](https://i.postimg.cc/sXBNJPsX/8.jpg)

### 我的 windows terminal 配置文件
```json
{
    "$help": "https://aka.ms/terminal-documentation",
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "actions": 
    [
        {
            "command": "paste",
            "keys": "ctrl+v"
        },
        {
            "command": 
            {
                "action": "copy",
                "singleLine": false
            },
            "keys": "ctrl+c"
        },
        {
            "command": "find",
            "keys": "ctrl+shift+f"
        },
        {
            "command": 
            {
                "action": "splitPane",
                "split": "auto",
                "splitMode": "duplicate"
            },
            "keys": "alt+shift+d"
        }
    ],
    "copyFormatting": "none",
    "copyOnSelect": false,
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
    "profiles": 
    {
        "defaults": 
        {
            "startingDirectory": null
        },
        "list": 
        [
            {
                "backgroundImage": "C:\\Users\\ASUS\\Pictures\\PowershellBG\\kalen.jpg",
                "backgroundImageOpacity": 0.7,
                "colorScheme": "cyberpunk",
                "font": 
                {
                    "face": "Cascadia Code"
                },
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "hidden": false,
                "name": "Windows PowerShell"
            },
            {
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "hidden": false,
                "name": "cmd"
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": true,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            },
            {
                "backgroundImage": "C:\\Users\\ASUS\\Pictures\\PowershellBG\\kalen.jpg",
                "backgroundImageOpacity": 0.7,
                "font": 
                {
                    "face": "DejaVu Sans Mono for Powerline"
                },
                "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                "hidden": false,
                "name": "Ubuntu",
                "source": "Windows.Terminal.Wsl"
            },
            {
                "backgroundImage": "C:\\Users\\ASUS\\Pictures\\PowershellBG\\kalen.jpg",
                "backgroundImageOpacity": 0.7,
                "colorScheme": "cyberpunk",
                "commandline": "D:\\Git\\Git\\bin\\bash.exe",
                "guid": "{11606d4b-0660-5aa9-861e-dfce500b7054}",
                "icon": "D:\\Git\\Git\\mingw64\\share\\git\\git-for-windows.ico",
                "name": "Git Bash"
            },
            {
                "backgroundImage": "C:\\Users\\ASUS\\Pictures\\PowershellBG\\benjamin.jpg",
                "backgroundImageOpacity": 0.8,
                "colorScheme": "cyberpunk",
                "font": 
                {
                    "face": "CaskaydiaCove Nerd Font"
                },
                "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
                "hidden": false,
                "name": "PowerShell 7",
                "source": "Windows.Terminal.PowershellCore",
                "startingDirectory": null
            }
        ]
    },
    "schemes": 
    [
        {
            "background": "#0C0C0C",
            "black": "#0C0C0C",
            "blue": "#0037DA",
            "brightBlack": "#767676",
            "brightBlue": "#3B78FF",
            "brightCyan": "#61D6D6",
            "brightGreen": "#16C60C",
            "brightPurple": "#B4009E",
            "brightRed": "#E74856",
            "brightWhite": "#F2F2F2",
            "brightYellow": "#F9F1A5",
            "cursorColor": "#FFFFFF",
            "cyan": "#3A96DD",
            "foreground": "#CCCCCC",
            "green": "#13A10E",
            "name": "Campbell",
            "purple": "#881798",
            "red": "#C50F1F",
            "selectionBackground": "#FFFFFF",
            "white": "#CCCCCC",
            "yellow": "#C19C00"
        },
        {
            "background": "#012456",
            "black": "#0C0C0C",
            "blue": "#0037DA",
            "brightBlack": "#767676",
            "brightBlue": "#3B78FF",
            "brightCyan": "#61D6D6",
            "brightGreen": "#16C60C",
            "brightPurple": "#B4009E",
            "brightRed": "#E74856",
            "brightWhite": "#F2F2F2",
            "brightYellow": "#F9F1A5",
            "cursorColor": "#FFFFFF",
            "cyan": "#3A96DD",
            "foreground": "#CCCCCC",
            "green": "#13A10E",
            "name": "Campbell Powershell",
            "purple": "#881798",
            "red": "#C50F1F",
            "selectionBackground": "#FFFFFF",
            "white": "#CCCCCC",
            "yellow": "#C19C00"
        },
        {
            "background": "#1F1F1F",
            "black": "#3A3D43",
            "blue": "#4F76A1",
            "brightBlack": "#368BFA",
            "brightBlue": "#428FFA",
            "brightCyan": "#2E706D",
            "brightGreen": "#0F722F",
            "brightPurple": "#FB0067",
            "brightRed": "#EB001F",
            "brightWhite": "#FDFFB9",
            "brightYellow": "#EEF107",
            "cursorColor": "#FFFFFF",
            "cyan": "#578FA4",
            "foreground": "#B9BCBA",
            "green": "#879A3B",
            "name": "DimmedMonokai",
            "purple": "#855C8D",
            "red": "#BE3F48",
            "selectionBackground": "#FFFFFF",
            "white": "#B9BCBA",
            "yellow": "#C5A635"
        },
        {
            "background": "#282C34",
            "black": "#282C34",
            "blue": "#61AFEF",
            "brightBlack": "#5A6374",
            "brightBlue": "#61AFEF",
            "brightCyan": "#56B6C2",
            "brightGreen": "#98C379",
            "brightPurple": "#C678DD",
            "brightRed": "#E06C75",
            "brightWhite": "#DCDFE4",
            "brightYellow": "#E5C07B",
            "cursorColor": "#FFFFFF",
            "cyan": "#56B6C2",
            "foreground": "#DCDFE4",
            "green": "#98C379",
            "name": "One Half Dark",
            "purple": "#C678DD",
            "red": "#E06C75",
            "selectionBackground": "#FFFFFF",
            "white": "#DCDFE4",
            "yellow": "#E5C07B"
        },
        {
            "background": "#FAFAFA",
            "black": "#383A42",
            "blue": "#0184BC",
            "brightBlack": "#4F525D",
            "brightBlue": "#61AFEF",
            "brightCyan": "#56B5C1",
            "brightGreen": "#98C379",
            "brightPurple": "#C577DD",
            "brightRed": "#DF6C75",
            "brightWhite": "#FFFFFF",
            "brightYellow": "#E4C07A",
            "cursorColor": "#4F525D",
            "cyan": "#0997B3",
            "foreground": "#383A42",
            "green": "#50A14F",
            "name": "One Half Light",
            "purple": "#A626A4",
            "red": "#E45649",
            "selectionBackground": "#FFFFFF",
            "white": "#FAFAFA",
            "yellow": "#C18301"
        },
        {
            "background": "#002B36",
            "black": "#002B36",
            "blue": "#268BD2",
            "brightBlack": "#073642",
            "brightBlue": "#839496",
            "brightCyan": "#93A1A1",
            "brightGreen": "#586E75",
            "brightPurple": "#6C71C4",
            "brightRed": "#CB4B16",
            "brightWhite": "#FDF6E3",
            "brightYellow": "#657B83",
            "cursorColor": "#FFFFFF",
            "cyan": "#2AA198",
            "foreground": "#839496",
            "green": "#859900",
            "name": "Solarized Dark",
            "purple": "#D33682",
            "red": "#DC322F",
            "selectionBackground": "#FFFFFF",
            "white": "#EEE8D5",
            "yellow": "#B58900"
        },
        {
            "background": "#FDF6E3",
            "black": "#002B36",
            "blue": "#268BD2",
            "brightBlack": "#073642",
            "brightBlue": "#839496",
            "brightCyan": "#93A1A1",
            "brightGreen": "#586E75",
            "brightPurple": "#6C71C4",
            "brightRed": "#CB4B16",
            "brightWhite": "#FDF6E3",
            "brightYellow": "#657B83",
            "cursorColor": "#002B36",
            "cyan": "#2AA198",
            "foreground": "#657B83",
            "green": "#859900",
            "name": "Solarized Light",
            "purple": "#D33682",
            "red": "#DC322F",
            "selectionBackground": "#FFFFFF",
            "white": "#EEE8D5",
            "yellow": "#B58900"
        },
        {
            "background": "#000000",
            "black": "#000000",
            "blue": "#3465A4",
            "brightBlack": "#555753",
            "brightBlue": "#729FCF",
            "brightCyan": "#34E2E2",
            "brightGreen": "#8AE234",
            "brightPurple": "#AD7FA8",
            "brightRed": "#EF2929",
            "brightWhite": "#EEEEEC",
            "brightYellow": "#FCE94F",
            "cursorColor": "#FFFFFF",
            "cyan": "#06989A",
            "foreground": "#D3D7CF",
            "green": "#4E9A06",
            "name": "Tango Dark",
            "purple": "#75507B",
            "red": "#CC0000",
            "selectionBackground": "#FFFFFF",
            "white": "#D3D7CF",
            "yellow": "#C4A000"
        },
        {
            "background": "#FFFFFF",
            "black": "#000000",
            "blue": "#3465A4",
            "brightBlack": "#555753",
            "brightBlue": "#729FCF",
            "brightCyan": "#34E2E2",
            "brightGreen": "#8AE234",
            "brightPurple": "#AD7FA8",
            "brightRed": "#EF2929",
            "brightWhite": "#EEEEEC",
            "brightYellow": "#FCE94F",
            "cursorColor": "#000000",
            "cyan": "#06989A",
            "foreground": "#555753",
            "green": "#4E9A06",
            "name": "Tango Light",
            "purple": "#75507B",
            "red": "#CC0000",
            "selectionBackground": "#FFFFFF",
            "white": "#D3D7CF",
            "yellow": "#C4A000"
        },
        {
            "background": "#000000",
            "black": "#000000",
            "blue": "#000080",
            "brightBlack": "#808080",
            "brightBlue": "#0000FF",
            "brightCyan": "#00FFFF",
            "brightGreen": "#00FF00",
            "brightPurple": "#FF00FF",
            "brightRed": "#FF0000",
            "brightWhite": "#FFFFFF",
            "brightYellow": "#FFFF00",
            "cursorColor": "#FFFFFF",
            "cyan": "#008080",
            "foreground": "#C0C0C0",
            "green": "#008000",
            "name": "Vintage",
            "purple": "#800080",
            "red": "#800000",
            "selectionBackground": "#FFFFFF",
            "white": "#C0C0C0",
            "yellow": "#808000"
        },
        {
            "background": "#332A57",
            "black": "#000000",
            "blue": "#00BFFF",
            "brightBlack": "#647DA1",
            "brightBlue": "#1BCCFD",
            "brightCyan": "#99D6FC",
            "brightGreen": "#21F6BC",
            "brightPurple": "#E6AEFE",
            "brightRed": "#FF8AA4",
            "brightWhite": "#FFFFFF",
            "brightYellow": "#FFF787",
            "cursorColor": "#FFFFFF",
            "cyan": "#86CBFE",
            "foreground": "#E5E5E5",
            "green": "#00FBAC",
            "name": "cyberpunk",
            "purple": "#DF95FF",
            "red": "#FF7092",
            "selectionBackground": "#FFFFFF",
            "white": "#FFFFFF",
            "yellow": "#FFFA6A"
        }
    ],
    "themes": []
}
```

## powershell 常用命令

>获取当前窗口命令历史记录。  
>```console
> Get-History
>```

>显示上一个、下一个命令。
>```console
> UpArrow DownArrow 方向键
>```

查找
`ctrl+shift+F`

> 查看powershell版本
>```console
> $PSVersionTable
>```

## 参考文章
[Oh My Posh：全平台终端提示符个性化工具](https://sspai.com/post/69911)
[安裝新版 Oh My Posh 與插件](https://www.kwchang0831.dev/dev-env/pwsh/oh-my-posh)
[有讲到Tabby](https://chaisw.cn/blog/1912.html)
[3，4步骤来源](https://blog.csdn.net/ahahayaa/article/details/125470204)

