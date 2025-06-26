# What to do with a new computer (win)

今天买了新电脑，配置它的时候不禁想整理一下这个问题. 

(以下文字全是个人意见.)



step 1: language pack downloading

第一步绝对是下载一下自己需要的 language pack. 首先打字爽了再说.



step 2: chrome

我觉得最主要的还是先下载个浏览器再说. 下载其他东西通常都需要经过浏览器. 并且 edge 看着真的很花里胡哨且累眼. 所以我会选择下一个 chrome, 然后 sync 一下我的账号.





step 3: steam (for wallpaper engine)

很 personal. 我的第三件事情是下 steam 然后下 wallpaper engine. 不用自己常用的壁纸真的很难使用这个电脑.



step 4: logi Ghub

调一下鼠标的灵敏度.



step 4: github desktop

个人习惯把很多 settings 的 config 文件都在 github desktop, 所以接下来会下一个 github desktop



step 5: wsl 下载

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



step 6: VSCode, typora, sublime

赶紧下个 VSCode 然后连接一下 wsl. 以及同步一下同系统之前保存好的 setting.json 文件

以及 typora for 查看之前的 notes, sublime for text editor in windows



step 7: 下一下 google drive 和 dropbox, 同步一下文件



step 8: 切个好的 shell. 

```
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





step 9: 配置一下 github 需要用到的 ssh

```shell
sudo apt install openssh-client
ssh-keygen -t ed25519 -C "rynnefan@umich.edu"
#Enter file in which to save the key (/home/rynne/.ssh/id_ed25519):
cat ~/.ssh/id_ed25519.pub
```

结果复制到 github 的 ssh

