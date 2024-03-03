---
title: Hexo at MacOS架站心路歷程：Part II - Hexo配置
date: 2024-03-03 17:51:31
# updated: 
tags: hexo
categories: Software
---

> Tags: [macOS](craftdocs://open?blockId=E2B93344-B0F1-4E5A-94AE-BB20FA32A655&spaceId=2e84309a-ca7c-d40a-c79f-c06a3542c138)

> 上一篇經歷Hexo的環境安裝，現在就讓我們進行基礎的Hexo配置。

## Step 1 - 初始化建立Hexo服務

- 首先，創立一個資料夾，我們會在這個資料夾中，建立服務

```other
mkdir <your-dir-name>
```

- 接著，初始化Hexo服務：

```other
hexo init <your_blog_name>
```

- 見到狀態：`INFO  Start blogging with Hexo!`表示已建立成功！

## Step 2 - 配置Hexo

> #### ！溫馨提醒！

- > 可以同時參照官方參考：[https://hexo.io/zh-cn/docs/configuration](https://hexo.io/zh-cn/docs/configuration)
- > 要稍微理解YAML的語法：冒號後面，需要一個**空格**才能繼續寫下去

> 這裡建議，直接參考官方的配置說明：[https://hexo.io/zh-cn/docs/configuration](https://hexo.io/zh-cn/docs/configuration)

- 如同官方所說，大部分的網頁配置，都是在`_config.yml` 或 [代替配置文件](https://hexo.io/zh-cn/docs/configuration#%E4%BD%BF%E7%94%A8%E4%BB%A3%E6%9B%BF%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6) 中調整修改

## Step 3 - 使用 Hexo 的指令

> 以下分享一些常用的Hexo指令，便於操作

> ### hexo init [folder]

- Goal: 初始化Hexo 的相關配置
- Parameters:
   - folder: 表是要初始化的文件資料夾地址
   - 若要直接在當前資料夾初始化，直接輸入`hexo init .`

> ### hexo server

- Goal: 可以在本地端進行網站的預覽，同時也是一種Debug方式
- 默認的網址連結為`http://localhost:8080/` ，可能會有所不同，像我就是4000這個port口
- 如果說想要換port地址，可以在終端器輸入`hexo s -p <new_port>`

> ### hexo generate

- Goal: 用於生成靜態文件，生成後網頁相關內容會放在根目錄下面的`public` 文件夾中

> ### hexo deploy

- Goal: 用來部署網站內容，使用這個命令會將生成好的頁面（根目錄下面的`public` 文件夾的內容）部署到指定的地方
- 這個指定的地方，可以在`_config.yml` 進行定義

> ### hexo clean

- Goal: 用於清空`public`文件夾內容

> ### hexo version

- Goal: 用於輸出目前hexo版本號

## Step 4 - 配置主題（我使用Keep）

> 我使用的主題為 [https://keep.xpoet.cn/](https://keep.xpoet.cn/)

- 如何設定：請參照[Keep 官網](https://keep-docs.xpoet.cn/basis/get-start/install-theme.html)的示範教學，依序設定

### 踩雷解決方法

1. 我用npm的方式下載好keep主題，但看不到相關的主題配置文件？
   1. 解法：在hexo根目錄中，Dir: `source` 資料夾建立資料夾 `_data` ，並且在該資料夾底下創建檔案`keep.yml` 。該主題就會讀取這檔案中的設定配置。
   2. 配置檔案設定路徑：  `source/_data/keep.yml`
   3. 再follow [https://keep-docs.xpoet.cn/basis/configuration-guide/base_info.html](https://keep-docs.xpoet.cn/basis/configuration-guide/base_info.html) 所述去配置。
2. 配置的「影像」檔案要放在哪裡？
   1. 文件中（.yml），會寫成`/images/xxx.svg`
   2. 檔案文件中會存放在：`source/images` 這個檔案中
3. 部署討論區時，該怎麼部署Disqus套件
   1. 請在主題設置文件中，輸入下列程式碼：
shortname的部分，可以參照下圖，登入Disqus後，尋找shortname的標籤
```yaml
comment:
  enable: true
  use: disqus  # values: valine | gitalk | twikoo | waline | giscus | artalk | disqus

  # Disqus Setting
  disqus:
    enable: true
    shortname: <your-shortname>
    count: true
```

![Image.png](https://res.craft.do/user/full/2e84309a-ca7c-d40a-c79f-c06a3542c138/doc/92EC70A5-36A8-4EB1-9E7B-E9A022328511/7ABBAA31-7678-40CA-AE9C-3E959D504E78_2/6RMxt4siGmWelKoZadcREm5DVyyVg5sbIw6j9yVpxnUz/Image.png)

