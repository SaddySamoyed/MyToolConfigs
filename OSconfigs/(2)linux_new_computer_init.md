## 0. 装个中文的输入法

最重要,,

我选择的是 google 拼音, 配合 Fcitx 框架.

```shell
sudo apt update
sudo apt install fcitx fcitx-googlepinyin fcitx-config-gtk
im-config -n fcitx
```

然后重启.

重启完之后启动配置工具

```shell
fcitx-config-gtk3
```

1. 点击左下角的 “+”
2. 搜索 “Google Pinyin”
3. 选中后添加

然后就可以用了. 切换 英文源和 Google Pinyin 是 Ctrl + Space, 在 Google pinyin 内切换英文是 shift.
### 切换为全部使用 half punc, 以及根据 frequency 来选 candidate

我的习惯: 更改为全部都用 half punc 而不是全标点.

方法:

打开 google pinyin 配置文件:
```shell
nano ~/.config/fcitx/conf/googlepinyin.conf
```

然后加上

```ini
[Punctuation]
FullWidthPunc=False
```

就好了.



然后: google 拼音是支持根据历史输入 frequency 来选词的. 我们 enable 它:

```shell
nano ~/.config/fcitx/conf/googlepinyin.conf
```

输入:

```ini
[General]
UseUserDict=True
AdjustOrder=True
SaveUserDictImmediately=True
```



### shift 切换中英文

```shell
fcitx-config-gtk3
```

在 **“全局配置 (Global Config)”** 找到一行叫：

```
Trigger Input Method
```

点击右边的快捷键栏, 然后按下 **Shift** 键即可.





## 1. 配置 ssh 来 clone github repo, 以及安装 github desktop linux 版

```sh
ssh-keygen -t ed25519 -C "rynnefan@gmail.com"
eval "$(ssh-agent -s)" 
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

得到 pub. 放进 github 配置

然后就可以 clone 自己的 git repos 了.



然后, 我的习惯是装一个 Github Desktop. 比起 GitKraken 它最大的优点就是可以操作 private 仓库 (GitKraken 居然操作个 private 仓库要付钱,,, 这已经是不可用了)

不过, 没有官方版本, 而是别人开源移植的.

在这里: https://github.com/shiftkey/desktop/releases/tag/release-3.4.13-linux1



(差点忘了, 得装个 git. `sudo apt install git`)



## 2. 安装 fish (shell) 和 kitty (terminal)

### fish

反正我不喜欢用 bash 和原生的 gnome terminal.

必要的配置都装上

fish:

```sh
sudo apt install fish
which fish
```

输出肯定是 `/usr/bin/fish`.

我们装好之后查看它在不在可用列表:

```shell
cat /etc/shells
```

不在的话加上:

```shell
which fish | sudo tee -a /etc/shells
```

然后设为默认:

```shell
chsh -s $(which fish)
```



### kitty

最主要的原因是原生的 gnome terminal 不支持选中文字时 ctrl c 不 kill process 而是复制, 以及 ctrl v. (可能可以自己配置, 我没了解过, 但是我知道 kitty 很好)

```shell
sudo apt install curl -y
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
kitty --version
```



Ubuntu 用 **`update-alternatives`** 管理默认终端程序:

```shell
sudo update-alternatives --config x-terminal-emulator
```

你会看到类似：

```
There are 3 choices for the alternative x-terminal-emulator (providing /usr/bin/x-terminal-emulator).

  Selection    Path                     Priority   Status
------------------------------------------------------------
* 0            /usr/bin/gnome-terminal   40        auto mode
  1            /usr/bin/gnome-terminal   40        manual mode
  2            /usr/bin/kitty            50        manual mode
```

输入 `2` (Kitty 对应编号) enter 就可以选择默认终端.
或者 pin 一下它.



我们可以配置它的外观:

```shell
code ~/.config/kitty/kitty.conf
```

config 方法在: https://sw.kovidgoyal.net/kitty/conf/

我的:

```config
# kitty.conf

font_family JetBrains Mono
font_size 12.5

cursor_text_color #111111
cursor_shape block

background_image ~/Documents/Github/MyToolConfigs/OSconfigs/wallpapers/02_6.jpg

# 图片布局 (常用 tiled, scaled, cover, contain)
background_image_layout tiled
background_image_linear yes    
background_opacity 1.0 
background #000000
foreground #eeeeee
background_tint 0.75  
background_tint_gaps 1.0


selection_foreground #000000
selection_background #fffacd

enable_audio_bell no

# 主题
include themes/nord.conf
```







## 4. VSCode, typora, sublime text 等

code:

```shell
sudo apt update
sudo apt install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt install apt-transport-https
sudo apt update
sudo apt install code
```

typora 和 sublime text 直接找官网 .deb 就行
