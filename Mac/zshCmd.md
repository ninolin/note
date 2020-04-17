# zsh command line 

## install iTerm2

安裝 iTerm2 成功後變更color scheme 為 xterm-256color

變更的操作位置在 Preferences > Profiles > Terminal > Report Terminal Type

## 安裝 zsh
```shell
# 安裝
$ brew install zsh
$ sudo sh -c "echo $(which zsh) >> /etc/shells"
$ chsh -s $(which zsh)
# 重啟 terminal
$ echo $SHELL
# 如果有成功變更shell為zsh的話會看到 /usr/local/bin/zsh
```

### 安裝 oh-my-zsh
```shell
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### 外掛 Auto Suggestions (for oh my zsh)
```shell
# 下載 zsh-autosuggestions
$ git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
or
$ git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
# 打開設定檔
$ open ~/.zshrc
# 在.zshrc檔當中找到 plugins=(git) -> 改為 plugins=(zsh-autosuggestions)
# 重啟 terminal
```
