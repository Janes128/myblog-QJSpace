---
title: Hexo at MacOS架站心路歷程：Part I - 環境安裝
date: 2024-02-29 22:22:48
tags: hexo
categories: Software
---

## 主旨與動機

- 想在繁忙的工作生活時間中，找到一個沈靜自己、消化自己收穫的小天地
- 用於記錄自己的工作、生活心得體悟
- 也分享給有需要的朋友們

## 心路歷程Ｉ：安裝Hexo必須環境(at MacOS)

> 學習連結：[https://easyhexo.com/1-Hexo-install-and-config/](https://easyhexo.com/1-Hexo-install-and-config/)

### Step 1 - 安裝套件管理工具：HomeBrew

> 官方連結：[https://brew.sh/zh-tw/](https://brew.sh/zh-tw/)

- 如同linux系統的`apt-get`，`brew`就是ＭＡＣ系統的套件管理工具
- 在電腦中的terminal執行下面指令

```other
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

- 讓他跑一下，過程中會需要輸入使用者密碼
- 接著，安裝好後，系統會提示下兩個指令，設定brew指令至PATH

```other
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/user/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

- 設定完指令後，即可測試指令`brew`是否可以運作！

### Step 2 - 安裝 git and Node.js

- 下載git套件，下指令安裝git，沒難度：

```other
brew install git
```

- 接著，下載套件Node.js，我們將透由`nvm`這個套件進行下載
   - [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

```other
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

- 注意：需要先重新啟動終端器，重啟後再輸入以下的指令，來安裝`npm`

```other
nvm install stable
```

- 可以輸入以下指令，看是否安裝成功：

```other
npm --version
```

### Step 3 - 安裝Hexo

- 最後，我們就可以使用npm指令來安裝我們的Hexo囉！

```other
npm install -g hexo-cli
```

## 小結

安裝的過程蠻簡單的，跟在linux上的使用體驗類似。

非常感謝各方大神的技術資源提供。

