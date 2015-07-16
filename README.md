Reactive Solar Farm Monitor
===========================

Reactive Solar Farm Monitor は [Typesafe Reactive Platform](http://www.typesafe.com/products/typesafe-reactive-platform) を利用したリアクティブシステムの具体例を示すため作成されたサンプル実装です。

リアクティブシステムとは？
--------------------------
下記の要件を満たすシステムがリアクティブシステムです。
* すばやい応答時間を保ち高いユーザービリティを実現
* 限りなく100%に近い稼働率を達成
* ワークロードが変動してもスケールアウト/スケールインが容易

詳細は[リアクティブ宣言](http://www.reactivemanifesto.org/ja)を参照してください。

Typesafe Reactive Platform とは？
---------------------------------
[Play Framework](https://playframework.com/)、[Akka](http://akka.io/)、[Scala](http://www.scala-lang.org/)、[Java](https://www.java.com/) を組み合わせることでリアクティブシステムを実現することができる開発基盤です。

詳細は [Typesafe Reactive Platform](http://www.typesafe.com/products/typesafe-reactive-platform) を参照してください。

概要
----
本サンプルシステムはソーラーファーム(大規模な太陽光発電所)に設置されたソーラーパネルの故障を検知するシステムを想定しています。

ソーラーファームには数万枚のソーラーパネルが設置されており、各ソーラーパネルにはそのパネルの発電量を逐次測定するデバイスが取り付けられています。
このデバイスが測定した発電量をもとにソーラーパネルの故障を検知できないでしょうか。
単にソーラーパネルの発電量が低下(ある閾値を下回る)したことを故障としてしまうと、悪天候でたまたま発電量が少なくなってしまった場合もソーラーパネルが故障したとみなされてしまいます。
そこで、このシステムではある瞬間における全ソーラーパネルの平均発電量と各ソーラーパネルの発電量を比較し、発電量が著しく平均を下回っているソーラーパネルを故障したとみなしています。

![abstract](img/reactive-solar-farm-monitor_abstract.png)

また、このシステムには達成しなければならない下記のような要件があります。

* ソーラーファームの発電効率を高めるため、ソーラーパネルの故障は1秒以内に検知できる
* 故障検知までのタイムラグが生じないように、稼働率は100%を達成する
* ソーラーファームの規模が拡大しソーラーパネルが増えた場合にスケールアウトできる

アーキテクチャ
--------------
本サンプルシステムは [Typesafe Reactive Platform](http://www.typesafe.com/products/typesafe-reactive-platform) を利用し、メッセージ駆動のアーキテクチャを採用しています。

![architecture](img/reactive-solar-farm-monitor_architecture.png)

スクリーンショット
------------------

![screenshot](img/reactive-solar-farm-monitor_screenshot.png)

起動方法
---------

### Docker を利用した起動

[Docker](https://www.docker.com/) がインストールされたPCで下記のコマンドを実行してください。

~~~
docker run -d --name=broker   -p 61613:61613                        crowbary/apache-apollo
docker run -d --name=solar_farm_simulator  --link=broker:broker     crowbary/reactive-solar-farm-monitor-solar-farm-simulator
docker run -d --name=analyzer -p 2551:2551 --link=broker:broker     crowbary/reactive-solar-farm-monitor-analyzer
docker run -d --name=monitor  -p 9000:9000 --link=analyzer:analyzer crowbary/reactive-solar-farm-monitor
~~~

ブラウザで http://[DOCKER_HOST]:9000/ へアクセスしてください。
* DOCKER_HOST: 上記の docker run コマンドを実行したホストのIPアドレスまたはホスト名

### Typesafe Activator を利用した起動

[Typesafe Activator](https://www.typesafe.com/get-started) を利用した起動方法を現在準備中です。

問い合わせ先
-------------
フィードバックや不明点等については下記までお問い合わせください。

TIS株式会社  
生産革新本部 生産革新部 生産技術R&D室  
リアクティブシステムコンサルティングサービス担当宛

<a href="mailto:go-reactive@tis.co.jp">go-reactive@tis.co.jp</a>

弊社は Typesafe Reactive Platform に関するコンサルティングサービスを提供しています。
コンサルティングサービスの概要は[こちら](http://www.tis.jp/service_solution/goreactive/)からご確認いただくことができます。

ライセンス
----------
Reactive Solar Farm Monitor は Apache License version 2.0 のもとにリリースされています。
Apache License version 2.0 の全文は[こちら](http://www.apache.org/licenses/LICENSE-2.0.html)からご覧いただくことができます。

---------

※ 記載されている会社名、製品名は、各社の登録商標または商標です。  
※ Icon made by [Freepik](http://www.freepik.com) from [www.flaticon.com](http://www.flaticon.com) is licensed under [CC BY 3.0](http://creativecommons.org/licenses/by/3.0/)

Copyright © 2015 TIS Inc.
