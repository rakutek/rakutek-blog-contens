---
title: "劣化具合を確認しながらwebp等に圧縮できるSquooshをshell scriptで自動化する"
emoji: "😼"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Squoosh, webp]
published: true
---

CyberAgent の[Web Speed Hackathon Online Vol.2](https://www.cyberagent.co.jp/careers/students/career_event/detail/id=25556))に参加した際に数 MB の画像がいくつもあってボトルネックになっていた物を webp 等に圧縮する時に使った Squoosh が便利だったので紹介します

![](https://storage.googleapis.com/zenn-user-upload/ysyz506qw43qjti8indqrzqjwp41)

## Squoosh 使い方

[Squoosh](https://squoosh.app/)は Google がオープンソースで開発している画像の圧縮やリサイズ、フォーマットの変換ができる Web アプリで、PWA でオフラインで使うことができます。

https://squoosh.app

https://github.com/GoogleChromeLabs/squoosh

使い方は非常のシンプルで圧縮したい画像をドラック&ドロップするとプレビュー画面に遷移するので、そこでリアルタイムに画質の劣化具合と圧縮率を確認しながら調節することができます

![](https://storage.googleapis.com/zenn-user-upload/jlo8gnphl77si2979tw8pv4ciuud)

## Cli 用のコマンドをエクスポートできる

Squoosh の欠点として、画像を１枚ずつしか圧縮できないのですが、Squoosh には Cli 版もあり、Web 版で画像でを圧縮した設定をコマンドとしてコピーすることができます...!

https://github.com/GoogleChromeLabs/squoosh/tree/dev/cli

プレビュー画面のコマンドラインアイコン?をクリックすることでクリップボードにコマンドがコピーされます
![](https://storage.googleapis.com/zenn-user-upload/ksu7l1mh69bb4wo78nkr9pu8qhqk)

これは一例です

```shell
npx @squoosh/cli --webp '{"quality":5.4,"target_size":0,"target_PSNR":0,"method":4,"sns_strength":50,"filter_strength":60,"filter_sharpness":0,"filter_type":1,"partitions":0,"segments":4,"pass":1,"show_compressed":0,"preprocessing":0,"autofilter":0,"partition_limit":0,"alpha_compression":1,"alpha_filtering":1,"alpha_quality":100,"lossless":0,"exact":0,"image_hint":0,"emulate_jpeg_size":0,"thread_level":0,"low_memory":0,"near_lossless":100,"use_delta_palette":0,"use_sharp_yuv":0}'
```

## Shell Script で自動化する

これでカレントディレクトリに存在する画像ファイルを Squoosh で劣化具合を確認した圧縮設定で圧縮することができます。

```shell
for FILE in ./*
    do
    FILENAME=`echo ${FILE}`
    squoosh-cli --resize '{"enabled":true,"width":887,"height":1331,"method":"lanczos3","fitMethod":"stretch","premultiply":true,"linearRGB":true}' --webp '{"quality":38.7,"target_size":0,"target_PSNR":0,"method":6,"sns_strength":50,"filter_strength":60,"filter_sharpness":0,"filter_type":1,"partitions":0,"segments":4,"pass":1,"show_compressed":0,"preprocessing":0,"autofilter":0,"partition_limit":0,"alpha_compression":1,"alpha_filtering":1,"alpha_quality":100,"lossless":0,"exact":0,"image_hint":0,"emulate_jpeg_size":0,"thread_level":0,"low_memory":0,"near_lossless":100,"use_delta_palette":0,"use_sharp_yuv":0}' ${FILENAME}
    done

```

```shell
1/1 ✔ Squoosh results:
 ./029b4b75-bbcc-4aa5-8bd7-e4bb12a33cd3.jpg: 5.61MB
  └ .webp → 110.66KB (1.93%)
1/1 ✔ Squoosh results:
 ./029b4b75-bbcc-4aa5-8bd7-e4bb12a33cd3.webp: 110.66KB
  └ .webp → 106.78KB (96.5%)
1/1 ✔ Squoosh results:
 ./078c4d42-12e3-4c1d-823c-9ba552f6b066.jpg: 6.38MB
  └ .webp → 146.70KB (2.25%)
1/1 ✔ Squoosh results:
 ./078c4d42-12e3-4c1d-823c-9ba552f6b066.webp: 146.70KB
  └ .webp → 143.24KB (97.6%)
1/1 ✔ Squoosh results:
 ./083258be-3e8c-4537-ac9c-fd5fd9cd943b.jpg: 694.98KB
  └ .webp → 12.96KB (1.86%)
1/1 ✔ Squoosh results:
 ./083258be-3e8c-4537-ac9c-fd5fd9cd943b.webp: 12.96KB
  └ .webp → 12.57KB (97.0%)
1/1 ✔ Squoosh results:
 ./18358ca6-0aa7-4592-9926-1ec522b9aacb.jpg: 4.22MB
  └ .webp → 94.81KB (2.19%)
0/1 ⠴ ▐▨▨▨▨▨╌╌╌╌╌▌ Encoding (12 threads)
```
