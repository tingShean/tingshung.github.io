---
title: "[CICD]Tekton in k8s 的第一步"
key: 20210426
layout: article
tags: cicd tekton k8s
---

## 前言

這套工具我相信有在接觸`Knative`的人應該都不會太陌生，最近因為一些案子的關係，所以我得把這東西導入，一方面是為了區分`CICD`的獨立性，二方面也是想試試這套好不好用。

<!--more-->

## 手忙腳亂的導入

原本我導入的版本是`v0.22.0`，然後順著教學往下做一切都很容易，但是在準備使用`private git`和`private dockerhub`的時候就出現問題了，首先`dockerconfig`還可以用別的方試實作，但是`git`這邊怎麼試就是沒辦法解決`.gitconfig`不存在的問題。

這邊要注意的是，`tekton`很多`secret`的東西都是透過`serviceAccount`來做為帳號管理，也就是你只要在`secret`設定好後，到 `serviceAccount`裡載入就可以在之後的`taskrun`以及`pipelinerun`裡來使用相關權限的登入。

最後我是直接退一版改用`v0.21.0`就解決上面這個問題了，足足卡了我一整個禮拜。


## 解決問題之後

解決了主要的敵人，次要敵人就會接二連三的出現；不過所幸都不是什麼太大的問題，相關的東西在社群討論裡都可以慢慢找到答案。

不過這邊主要是記載一些在導入的時候比較麻煩的點，所以並沒有什麼太多深入的地方，在一般`tekton`設定的`task`和`pipeline`這邊來說，`pipeline`可以直接寫`task`(~~廢話~~)，也就是說我可以不需要先寫好`task`就直接寫`pipeline`是可以的，但是為了讓自己好做事我個人還是建議人家教學怎麼教，你就怎麼學，不然整體上維護的時間會花在很多不必要的細節裡。


## 方便性

`tekton`在越寫越順之後，會發現他真的是把`k8s`當作一個`stack`在存放那些可以一直被`reuse`的`task`，也就是說今天你建了一些`task`是為了'`nodejs`、`php`、`python`或是`Golang`以及`java`不管怎樣，你只要把相關的`build`寫成`task`放在`k8s`裡，日後要用只需要在`pipeline`裡呼叫對應的執行動作就可以了，而官方的`github`也提供了很多你想的到的和你沒想到的`build`檔相關的執行程序供你使用。


## 結語

這篇東西不多，主要是注意一下使用版本的問題，如果後續的版本也沒有上述的兩個`config`檔的話，應該也有相關的作法可以參考，下一篇來談談`trigger`；這個才是我花最久時間的地方。


## 參考資料

[Tekton GitHub](https://github.com/tektoncd/pipeline)
