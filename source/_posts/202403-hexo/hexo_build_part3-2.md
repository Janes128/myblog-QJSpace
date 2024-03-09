---
title: Hexo at MacOS架站心路歷程：Part III - Hexo部署架設 - Netlify + Github 篇
date: 2024-03-09 23:21:27
updated:
tags: hexo
categories: Software
---

## 動機與目的

- 用了Github Pages部署方式後，發現會有些不方便的地方，比如更新文章時，需要在本地端先進行編譯建置（hexo generate），部署到public文件夾後，再手動上傳資料夾中內容至github更新網頁。
- 上網研究，發現**Netlify**這項好用工具，可以更快速方便的更新自己的網站，做到**持續性部署（CD）**
- 接下來，就讓我們開始吧！

## 使用Netlify＋Github進行網站部署

> Netlify 是一家遠端優先的雲端運算公司，提供一個開發平台，其中包括針對 Web 應用程式和動態網站的建置、部署和無伺服器後端服務。(Wiki)

- 操作原理
   - 將Hexo 部落格相關資料，丟到Github建立一個repo
   - 使用Netlify來建立無伺服器的後端部署服務，並連結至上述repo
   - Netlify將會自動偵測Github repo的更新變動，自動進行部署

### Step 1: 將Hexo Project上傳github進行遠端管理

- 在部落格的根目錄中，初始git服務，並將我的hexo網頁檔案push上github repo

```bash
$ git init
$ git add .
$ git commit -m ":tada: Init repo"
$ git remote add origin git@github.com:<your-github-name>/<hexo-project-name>.git
$ git push -u origin master #here need PAT to login your github
```

> #### Debug - git problem with 'critical error: 身份驗證失敗'

> 請參考這篇：[https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed](https://stackoverflow.com/questions/68775869/message-support-for-password-authentication-was-removed)

> - 2021-08-13以後，github不再支援直接輸入密碼的方式，來操作git；而是使用**PAT (Personal Access Token)**作為替代。
> - 長話短說：請至github頁面申請一組PAT，使用PAT來取代密碼即可。

### Step 2: 辦理與部署Netlify

> 官網：[https://www.netlify.com/](https://www.netlify.com/)

- 登入官網後，可以直接使用github帳號做登陸
- 填寫完相關資訊，請選擇使用github連動您的hexo repo(剛剛上傳的那個)

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/2FD6B68B-6A84-4234-80CE-E0CBD0D7BF82/EBC2E25C-0475-430C-9B7E-184BA3EA2870_2/fe0JMSwyNbBg6wARRtS33ooYrPHvzrNqpdRlD4Vjkogz/Image.png)

- 點選剛剛連動的repo，再點選Edit build settings

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/2FD6B68B-6A84-4234-80CE-E0CBD0D7BF82/853FD916-ADE3-4923-89A8-2FE8E386C11D_2/hhux99NeYzpKALvcm9nBGnSoc2NkraodIn9HQsVlfvMz/Image.png)

- 修改如下圖所示(其實使用系統預設值即可)

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/46342A65-FCCC-4ABE-A88C-54FA91036167/20A9448C-164D-42C8-A10A-005B69E0DD0A_2/nIwQiXx6rLUFQZzc27MQenuWSyxQiNn0jyF5Ltyix0kz/Image.png)

- 修改後，即可按下部署「Deploy name to Netlify」並等待發出；成功的話，將會出現下面的資訊囉！

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/46342A65-FCCC-4ABE-A88C-54FA91036167/F0266889-0F9B-424A-8F05-253C80571EE8_2/yLZc0NJzxfOLXY2qnNyBChBAdgUKYJN2D9hIzEc0yCEz/Image.png)

- 此時，就可以接著後續的設定，如：客製化網誌ＵＲＬ，或是內嵌入其他插件，這邊不做贅述。
- 如此一來，就完成網站的部署囉！

---

## Debug經驗分享：Netlify 部署Hexo Github 初始化（initializing） 失敗

- 問題發現：
   - 建制初期，進行Hexo部落格更新時，推到Github上，理論上Netlify會自動幫我編譯與部署，但當netlify在進行初始化(initializing)動作，會跳出**Failed**的警告字樣，如下圖：

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/46342A65-FCCC-4ABE-A88C-54FA91036167/D0688305-FD1E-430A-ABED-133DD3C8E5FF_2/f8M3qyROpcUFeVybxfwylg6Tbpq9pIfbI9qTSO8lSmkz/Image.png)

- 分析觀察：
   - 觀察第八與第九行，可以發現，問題是出在「.deploy_git」這個檔案。發現這個檔案應該是Netlify在部署時會自動產生出的文件，git submodule 無法追蹤或更新到這份模組，導致錯誤。
- 解決方法：
   - 參考以下連結，先將「.deploy_git」這個檔案給刪掉

[](https://blog.csdn.net/lemqs0123/article/details/110429731)

   - 刪除指令：
```bash
git rm --cache '.deploy_git'    # system response: rm '.deploy_git'
```

   - 下圖為檢查與刪除的過程圖：

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/46342A65-FCCC-4ABE-A88C-54FA91036167/92A29C99-9794-49BA-A79D-C64090A53662_2/fTUgJOlycKrHdenqq0b8y7qnYImjkE7WJvSMp2G1ipQz/Image.png)

   - 並且，記得將部署檔案".deploy_git"加入到".gitignore"檔案裡面（若無此檔案，要自己創建），以免再次把".deploy_git"更新上去。
      - 可以使用指令`ls -la` 看看有沒有這個檔案，若沒有，可以用指令`touch .gitignore` 來創造，再使用文字編輯器將檔案`.gitignore` 加上去即可
   - 編輯結束後，再將hexo的repo推上去github，讓netlify自動部署更新，就可以成功囉！

---

## 小結

- 將部署工具換成netlify之後，有種如釋負重的感覺，之前繁瑣的部署流程，可以直接推上github後就結束了，他將會幫我自動部署到網路上，輕鬆方便許多！
- 遇到Bug時，一時間也覺得不知所措，但真的仔細深入了解其原因後，才發現原來解法這麼單純，但這也是玩資訊工程時的醍醐味吧！

