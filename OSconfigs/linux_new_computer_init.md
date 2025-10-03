### 0. 装个中文的 input source 

记得是 pinyin



### 1. 首先配置 ssh 来 clone github repo

```sh
ssh-keygen -t ed25519 -C "rynnefan@gmail.com"
eval "$(ssh-agent -s)" 
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

得到 pub. 放进 github 配置



### 2. 安装 git, fish 和 VSCode, typora 等

反正我不喜欢用 vim 和 bash

必要的配置都装上

fish:

```sh
sudo apt install fish
which fish
```



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



