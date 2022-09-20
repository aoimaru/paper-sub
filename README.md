## はじめに
管理コストや計算資源の負荷を削減することを目的として, 近年コンテナ型の仮想化技術が広く利用されている．現在, コンテナ型の仮想化技術のデファクトスタンダードとしてDockerが位置づいており, Dockerを用いることで開発者はアプリケーションをインフラストラクチャから切り離すことができる[].
Dockerではコンテナを構築する方法が複数用意されており, その中のひとつにDockerfileを用いた方法がある. Dockerfileとはコンテナの構築情報を手続き的に記述したファイルであり, この中の情報を逐次的に実行していくことでコンテナランタイムの実行環境をビルドすることができる. Dockerfileのようにインフラストラクチャの構築情報をコードとして管理することをInfrastructure as Code(IaC)と呼び, IaCを活用することでインフラストラクチャのバージョン管理や共有, 配布などといったことが可能になる[].
しかしながら, Dockerfileなどを含めたIaCは, 比較的新しい技術分野であり, 静的解析や開発支援などにおいて, 研究が未熟な領域が存在していることが指摘されている[引用].![image](https://user-images.githubusercontent.com/51444995/190901121-f1817aca-0ace-4e2e-b699-837f67a2d756.png)



## 

```bash

RUN ... && \
    wget -O [obj.tar.xz] [URL] && \
    echo " *** obj.tar.xz" | sha256sum -c && \
    mkdir -p [directory/obj] && \
    tar -xJf [obj.tar.xz] -C [directory/obj] && \
    rm [obj.tar.xz] && \
    cd [directory/obj]
    ...
    
    
RUN ... && \
    wget -O [obj.tar.xz] [URL] && \
    echo " *** obj.tar.xz" | sha256sum -c && \
    tar -xJf [obj.tar.xz] -C [directory/obj] && \
    mkdir -p [directory/obj] && \
    rm [obj.tar.xz] && \
    cd [directory/obj]
    ...

```
