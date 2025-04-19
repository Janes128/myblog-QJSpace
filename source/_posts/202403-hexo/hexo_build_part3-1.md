---
title: Hexo at MacOS架站心路歷程：Part III - Hexo部署架設 - Github Pages 篇
date: 2024-03-09 13:00:40
updated:
tags: hexo
categories: software
---

> 上一篇經歷Hexo的設定配置，現在就讓我們進行Hexo的部署架設任務，期待我們的網站出現在網路世界上！

## 使用Github Pages進行網站部署

### 申請Github Pages

- 因為我很早以前申請過了><，網路上也有各方豪傑的分享可以參考，這邊就先跳過，假設申請好了！
- 請記下git申請過後的網址：[`https://github.com/your_username/your_reponame.git`](https://github.com/your_username/your_reponame.git)

### 於本地端建立Github Pages服務

- 首先，安裝相關的部署套件

```other
$ npm install hexo-deployer-git --save
$ npm install hexo-server --save
```

- 填寫git的相關標註資訊

```other
$ git config --global user.name "Your_user_name"
$ git config --global user.email Your_email@example.com
```

- 接下來，設定部落格中`_config.yml`的部署文件

```yaml
deploy:
  type: git
  repo: <your-git-repo-url>
  branch: master
  message:  # Default is deploy time
```

- 最後，使用以下指令，將網站生成並部署到github上

```yaml
$ hexo clean && hexo d -g
```

- 如此一來，只要輸入你的github pages網址：[https://<your-name>.github.io/](https://janes128.github.io/)，就可以看到你自己架設的網站囉！可喜可賀！

---

### 小結

第一次部署Hexo網站，可以在無邊際的網路海中，看到自己的一點足跡，非常感動！也期勉自己，可以維持這樣的恆心毅力，分享自己的生活工作，希望可以對別人有所幫助！

