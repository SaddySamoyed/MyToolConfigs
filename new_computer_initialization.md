# What to do with a new computer (win)

今天买了新电脑，配置它的时候不禁想整理一下这个问题. 

(以下文字全是个人意见.)



### step 1: language pack downloading

第一步绝对是下载一下自己需要的 language pack. 首先打字爽了再说.



### step 2: chrome

我觉得最主要的还是先下载个浏览器再说. 下载其他东西通常都需要经过浏览器. 并且 edge 看着真的很花里胡哨且累眼. 所以我会选择下一个 chrome, 然后 sync 一下我的账号.





### step 3: steam (for wallpaper engine)

很 personal. 我的第三件事情是下 steam 然后下 wallpaper engine. 不用自己常用的壁纸真的很难使用这个电脑.

btw 启动覆盖 Lock screen + 允许 sleep 时运行还可以解锁覆盖屏保小连招. 不过刚开机的时候不会启动，还得同时自己设一个文件夹 slide.

### step 4: logi GHub

调一下鼠标的灵敏度.



### step 4: github desktop

个人习惯把很多 settings 的 config 文件都在 github desktop, 所以接下来会下一个 github desktop



### step 5: wsl 下载

最重要的就是先把 wsl 下了

```shell
wsl --install
```

然后赶紧切 bash 下载指令

```shell
sudo apt update
sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
sudo apt install g++-13 make rsync wget git ssh gdb tree
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-13 100
sudo update-alternatives --config g++
```



### step 6: VSCode, typora, sublime

赶紧下个 VSCode 然后连接一下 wsl. 以及同步一下同系统之前保存好的 setting.json 文件

以及 typora for 查看之前的 notes, sublime for text editor in windows



顺便, 更改一下注册表里的可新建类型, 右键新建更快一点.

比如 md: 新建一个 `something.reg` 文件, 放入这个然后双击运行一下就好了

```reg
Windows Registry Editor Version 5.00

[-HKEY_CLASSES_ROOT\.md]  ; 清除原有 .md 注册信息

[HKEY_CLASSES_ROOT\.md]
@="Markdown.File"
"Content Type"="text/markdown"

[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""

[HKEY_CLASSES_ROOT\Markdown.File]
@="Markdown File"

[HKEY_CLASSES_ROOT\Markdown.File\ShellNew]
"NullFile"=""

```









### step 7: 下一下 google drive 和 dropbox, 同步一下文件

cloud drive 可以同步一下老电脑的 desktop 和 documents



### step 8: 切个顺手的 shell

我切下 fish

```shell
sudo apt install fish
chsh -s /usr/bin/fish
```

然后重新打开 shell, 现在是 fish, 首先安装一下 VSCode in Ubuntu (以便可以 apt 运行)

(

```shell
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code
```

)

然后:

```shell
set -gx PATH /usr/bin /usr/local/bin $PATH
sudo code ~/.config/fish/config.fish
```





### step 9: 配置一下 github 需要用到的 ssh

```shell
sudo apt install openssh-client
ssh-keygen -t ed25519 -C "rynnefan@umich.edu"
#Enter file in which to save the key (/home/rynne/.ssh/id_ed25519):
cat ~/.ssh/id_ed25519.pub
```

结果复制到 github 的 ssh



### stp10: 配置一下常用 scripting language 的环境

scripting language 还是非常必要的

我习惯用 py

我这里在 windows 和 wsl 都下一个 miniconda 的包管理器

Windows:

```shell
wget "https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe" -outfile ".\Downloads\Miniconda3-latest-Windows-x86_64.exe"
```

```shell
~/Miniconda3-latest-Linux-x86_64.sh
```



Linux:

```shell
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

```shell
bash ~/Miniconda3-latest-Linux-x86_64.sh
# Miniconda3 will now be installed into this location: /home/rynne/miniconda3
```

在 config 文件里:

```shell
set -gx PATH /home/rynne/miniconda3/bin $PATH
```

然后 source 一下. 

然后可以创建环境

```shell
conda create -n myenv python=3.12
```

查看:

```shell
conda env list
```



而 windows 的直接用 anaconda powershell 就行.



还可以下载额外 GUI:

```shell
conda install jupyterlab
conda install anaconda-navigator
```



 

### step 11: 其他常用软件

我的话会下一个 mathpix 快速公式转 latex/md 的工具

以及一个 powertoy, 重新映射一下键, 把 f6 映射到 printscreen

(powertoy 真挺有用的. 因为比如 rog 幻 16 的原生键盘布局是没有 printscreen 的.)

然后把灯光什么的调一下

以及下个 custom cursor 




### step 12: memory management

内存管理. 还是挺重要的

主要有些厂商开机占有率太高了. 大概率是一些服务 start up 的原因, 首先少开点 app 上的 startup (比如 dropbox) 其次对系统的 startup processes 可以用 RamMap 清一下

我会:

1. 给 wsl 分配指定的内存上限. 不能太高了

2. 下载一个 RamMap 管理内存

3. 下载一个 autohotkey 然后把这一段 `launch_gamebar.ahk` 的 快捷方式放在 shell:startup 里面 (winR 打开输入 shell:shartup)

   ```ahk
   #Persistent
   SetTimer, LaunchGameBar, 5000  ; 等待5秒后执行（确保系统加载完）
   Return
   
   LaunchGameBar:
   SetTimer, LaunchGameBar, Off
   Send, #{g} ; 发送 Win+G
   ExitApp
   
   ```

   目的是用 gamebar 来检测性能.

4. 启动一下独显直连. 核显还是不用好

这样我的开机时内存占用率就从 60% 降低到了 25%. 
