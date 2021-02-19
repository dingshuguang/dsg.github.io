---
layout: post
title: MIDI键盘宿主采样器的安装使用
date: 2021-02-19
tags: cakewalk
top_img: https://res.cloudinary.com/ddsg/image/upload/c_scale,w_1920/v1613697344/ebuen-clemente-jr-H5Iw3Xz0vxM-unsplash_qbudyy.jpg
cover: https://res.cloudinary.com/ddsg/image/upload/c_scale,w_1920/v1613697344/ebuen-clemente-jr-H5Iw3Xz0vxM-unsplash_qbudyy.jpg
---

很長時間以前，我買了一個 MIDI 鍵盤 NEKTAR GX61 一直沒有仔細的使用過，直到現在我在家裏有時間就找出來，研究了幾天。 MIDI 鍵盤本身不發聲音，需要藉助音源來出聲。而音源不能直接和鍵盤聯係上，需要藉助一個采樣器，就是 kontakt  宿主軟件cakewalk就是編輯音頻用的，可以在這裏錄製，剪輯音頻。而這兩款軟件現在全部是免費的。  

kontakt 現在已經到了6.5.1 的版本了，點擊[這裏](https://www.native-instruments.com/zh/products/komplete/samplers/kontakt-6-player/)下載 native access 需要注冊一個賬戶，然後打開后在 not installed 裏可以找到 kontakt 安裝就可以了，這裏還有很多音色也挺有意思的，比如 kontakt factory selection 和 play series selection 。  

cakewalk 則需要下載 [bandlab](https://www.bandlab.com/products/desktop/assistant) ，在 apps 裏找到 cakewalk 點擊安裝。  

如果想要在 cakewalk 裏調用 kontakt 采樣器，其實如果不需要錄製，是沒有必要調用的。調用時需要把 kontakt 的 dll 文件放到自己創建的插件文件夾下，然後啓動 cakewalk 的時候就會掃描到 kontakt 樂器，同時還要在 cakewalk 裏參數設置 - VST設置 - vst掃描路徑裏添加自己創建的插件文件夾路徑，重啓。  

還有一個就是 MIDI 鍵盤的設置問題。需要下載個設置配置文件，到[官網](https://nektartech.com/)這裏，先注冊賬戶，輸入自己設備的序列號，然後根據自己使用的宿主軟件下載適應的設置驅動程序。  

在使用 kontakt 的時候，如果有很多個樂器，它自動會分配通道，這樣鍵盤要想更換樂器的時候需要更換通道才能使其他的樂器發聲， GX61 是按下 set 鍵，再按一下 global MIDI CH 鍵，再按一下右邊的想要切換的通道的數字鍵盤，最後確認。