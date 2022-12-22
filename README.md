

Dockerfile アンチパターン \

@1: aptGetUpdatePrecedesInstall \
@2: DL3009:aptGetInstallRmAptLists \
(@3: DL3019:apkAddUseNoCache) \
@4: gpgVerifyAscRmAsc \
~~@5: original:usePackagePrecedesAptInstall~~\
@6: rmRecurisveAfterMktempD \
@7: tarSomethingRmTheSomething

```bash


RUN curl -sSL "http://www.polishmywriting.com/download/atd_distribution${ATD_VERSION}.tgz" -o /tmp/atd.tar.gz \
	&& mkdir -p /usr/src/atd \
	&& tar -xzf /tmp/atd.tar.gz -C /usr/src/atd --strip-components 1 
	

RUN curl -sSL "http://www.polishmywriting.com/download/atd_distribution${ATD_VERSION}.tgz" -o /tmp/atd.tar.gz \
	&& mkdir -p /usr/src/atd \
	&& tar -xzf /tmp/atd.tar.gz -C /usr/src/atd --strip-components 1 \
	&& rm /tmp/atd.tar.gz*


RUN set -x \
	&& apk add --no-cache --virtual .build-deps \
		build-base \
		curl \
		libressl-dev \
		perl-dev \
		tar \
	&& curl -sSL "http://znc.in/releases/znc-${ZNC_VERSION}.tar.gz" -o /tmp/znc.tar.gz \
	&& mkdir -p /usr/src/znc \
	&& tar -xzf /tmp/znc.tar.gz -C /usr/src/znc --strip-components 1 \
	&& rm /tmp/znc.tar.gz* \
	&& ( \
		cd /usr/src/znc \
		&& ./configure \
		&& make -j8 \
		&& make install \
	) \
	&& rm -rf /usr/src/znc \
	&& runDeps="$( \
		scanelf --needed --nobanner --recursive /usr \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)" \
	&& apk add --no-cache --virtual .irssi-rundeps $runDeps \
	&& apk del .build-deps


C1: RUN curl -sSL "http://www.polishmywriting.com/download/atd_distribution${ATD_VERSION}.tgz" -o /tmp/atd.tar.gz \
D1: 	&& mkdir -p /usr/src/atd \
E1: 	&& tar -xzf /tmp/atd.tar.gz -C /usr/src/atd --strip-components 1 \

A2: RUN...
C2: 	&& curl -sSL "http://znc.in/releases/znc-${ZNC_VERSION}.tar.gz" -o /tmp/znc.tar.gz \
D2: 	&& mkdir -p /usr/src/znc \
E2: 	&& tar -xzf /tmp/znc.tar.gz -C /usr/src/znc --strip-components 1 \
F2: 	&& rm /tmp/znc.tar.gz* \

LD(CDE -> ABCDEF)の編集回数

INSERT
LD(CDE->ABCDE)+Fの追加 -> LD(CDE->ABCDEF)=(2+1)

DELETE
Eの削除+LD(CD->ABCDEF) -> LD(CDE->ABCDEF)=(4+1)

REPLACE
EをFに置換+LD(CD-ABCDE) -> LD(CDE->ABCDEF)=(3+1)


LD(CDE -> ABCDE)の編集回数
REPLACE
Eを置換+LD(CD-ABCD) -> LD(CDE->ABCDE)=(2+0)

D1==D2の判定などにベクトルや正規圧縮距離を利用



S1: CDE
S2: ABCDEFGH
Replace:1 ADD:1 DELETE:1
╒═════╤═══════╤═════╤═════╤══════╤══════╤══════╤═════╤═════╤═════╕
│     │   j-> │   A │   B │   C~ │   D~ │   E~ │   F │   G │   H │
╞═════╪═══════╪═════╪═════╪══════╪══════╪══════╪═════╪═════╪═════╡
│ i-> │     0 │   1 │   2 │    3 │    4 │    5 │   6 │   7 │   8 │
├─────┼───────┼─────┼─────┼──────┼──────┼──────┼─────┼─────┼─────┤
│ C   │     1 │   1 │   2 │    2 │    3 │    4 │   5 │   6 │   7 │
├─────┼───────┼─────┼─────┼──────┼──────┼──────┼─────┼─────┼─────┤
│ D   │     2 │   2 │   2 │    3 │    2 │    3 │   4 │   5 │   6 │
├─────┼───────┼─────┼─────┼──────┼──────┼──────┼─────┼─────┼─────┤
│ E   │     3 │   3 │   3 │    3 │    3 │    2 │   3 │   4 │   5 │
╘═════╧═══════╧═════╧═════╧══════╧══════╧══════╧═════╧═════╧═════╛
cost: 0.625

C1: RUN curl -sSL "http://www.polishmywriting.com/download/atd_distribution${ATD_VERSION}.tgz" -o /tmp/atd.tar.gz \
D1: 	&& mkdir -p /usr/src/atd \
E1: 	&& tar -xzf /tmp/atd.tar.gz -C /usr/src/atd --strip-components 1 \

A2: RUN set -x \
B2: 	&& apk add --no-cache curl tar \
C2: 	&& curl -sSL "http://znc.in/releases/znc-${ZNC_VERSION}.tar.gz" -o /tmp/znc.tar.gz \
D2: 	&& mkdir -p /usr/src/znc \
E2: 	&& tar -xzf /tmp/znc.tar.gz -C /usr/src/znc --strip-components 1 \
F2: 	&& rm /tmp/znc.tar.gz* \
G2: 	&& apk add --no-cache --virtual .irssi-rundeps $runDeps \
H2: 	&& apk del .build-deps


A2: RUN set -x \
B2: 	&& apk add --no-cache curl tar \
C2: 	&& curl -sSL "http://znc.in/releases/znc-${ZNC_VERSION}.tar.gz" -o /tmp/znc.tar.gz \
D2: 	&& mkdir -p /usr/src/znc \
E2: 	&& tar -xzf /tmp/znc.tar.gz -C /usr/src/znc --strip-components 1 \
F2: 	&& rm /tmp/znc.tar.gz* \
G2: 	&& apk add --no-cache --virtual .irssi-rundeps $runDeps \
H2: 	&& apk del .build-deps

単体のインデックス: [A2, B2, C2, D2, E2, F2, G2, H2]
複数のインデックス: [[A2, B2], [B2, C2], ..., [B2, C2, F2, H2], ...]







@@ -2,4 +2,4 @@ FROM debian

 RUN apt-get update && \
     apt-get install -y --no-install-recommends \
-        python
\ No newline at end of file
+        python==3.8.1
\ No newline at end of file



commit 8f05504e1c32d3b70683890a2e809ae9374f3841 (HEAD -> master)
Author: aoimaru <51444995+aoimaru@users.noreply.github.com>
Date:   Wed Nov 30 11:54:40 2022 +0900

    [refactoring] DL3008の匂いを修正
    /** Hadolintでの匂いの検知結果 */
    -:1 DL3006 warning: Always tag the version of an image explicitly
    -:3 DL3009 info: Delete the apt-get lists after installing something

commit f102d30f9b78bf4cb40cc03cf5aa9ea7b2d7d8db
Author: aoimaru <51444995+aoimaru@users.noreply.github.com>
Date:   Wed Nov 30 11:53:32 2022 +0900

    create Dockerfile
    /** Hadolintでの匂いの検知結果 */
    -:1 DL3006 warning: Always tag the version of an image explicitly
    -:3 DL3008 warning: Pin versions in apt get install. Instead of `apt-get install <package>` use `apt-get install <package>=<version>`
    -:3 DL3009 info: Delete the apt-get lists after installing something

commit: "create Dockerfile"  --> commit: "[refactoring] DL3008の匂いを修正"
[delete]: -:3 DL3008 warning: Pin versions in apt get install. Instead of `apt-get install <package>` use `apt-get install <package>=<version>`


```


```bash


commit 01396da224563d26dcdd04a9f7648861f7b3d631 (HEAD -> master, origin/master, origin/HEAD)
Author: Trong Nghia Nguyen <nghiant2710@gmail.com>
Date:   Wed Nov 9 11:09:14 2022 +0000

    Autogenerated Dockerfiles

    Signed-off-by: Trong Nghia Nguyen <nghiant2710@gmail.com>

commit 88ed1156e6bf54e9662796cf1d573e727cd9e9e9
Merge: 779436e9e9ee e96821d91b26
Author: Trong Nghia Nguyen <nghiant2710@gmail.com>
Date:   Wed Nov 9 14:44:12 2022 +0700

    Merge pull request #800 from balena-io-library/contracts-update

    Update contracts to v2.0.28

commit e96821d91b26662d8dd090bb92e38e064bcdcfad
Author: Trong Nghia Nguyen <nghiant2710@gmail.com>
Date:   Wed Nov 9 07:40:35 2022 +0000

    Update contracts to v2.0.28

    Change-type: patch
    Signed-off-by: Trong Nghia Nguyen <nghiant2710@gmail.com>

commit 779436e9e9eeed818be9b55688311b3c8eccc002
Author: Trong Nghia Nguyen <nghiant2710@gmail.com>
Date:   Mon Nov 7 15:24:02 2022 +0000

    Autogenerated Dockerhub descriptions

    Signed-off-by: Trong Nghia Nguyen <nghiant2710@gmail.com>

commit a581d0ae225f0d5a56ed12c9899836d69a1e4bf9
Author: Trong Nghia Nguyen <nghiant2710@gmail.com>
Date:   Mon Nov 7 14:07:08 2022 +0000

```






```bash

gpg --batch --verify openjdk.tgz.asc openjdk.tgz; \


{
    "children": [
	{
	    "children": [],
	    "type": "SC-GPG-F-BATCH"
	},
	{
	    "children": [
		{
		    "children": [
			{
			    "children": [
				{
				    "type": "ABS-EXTENSION-ASC",
				    "children": []
				}
			    ],
			    "type": "BASH-LITERAL"
			}
		    ],
		    "type": "SC-GPG-VERIFY"
		},
		{
		    "children": [
			{
			    "children": [],
			    "type": "BASH-LITERAL"
			}
		    ],
		    "type": "SC-GPG-VERIFY"
		}
	    ],
	    "type": "SC-GPG-VERIFYS"
	}
    ],
    "type": "SC-GPG"
}
```




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



# set up nsswitch.conf for Go's "netgo" implementation (which Docker explicitly uses)
# - https://github.com/docker/docker-ce/blob/v17.09.0-ce/components/engine/hack/make.sh#L149
# - https://github.com/golang/go/blob/go1.9.1/src/net/conf.go#L194-L275
# - docker run --rm debian:stretch grep '^hosts:' /etc/nsswitch.conf
RUN [ ! -e /etc/nsswitch.conf ] && echo 'hosts: files dns' > /etc/nsswitch.conf

ENV DOCKER_CHANNEL test
ENV DOCKER_VERSION 19.03.0-rc3
# TODO ENV DOCKER_SHA256
# https://github.com/docker/docker-ce/blob/5b073ee2cf564edee5adca05eee574142f7627bb/components/packaging/static/hash_files !!
# (no SHA file artifacts on download.docker.com yet as of 2017-06-07 though)

RUN set -eux; \
	...
	if ! wget -O docker.tgz "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; then \
		echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${dockerArch}'"; \
		exit 1; \
	fi; \
	tar --extract \
		--file docker.tgz \
		--strip-components 1 \
		--directory /usr/local/bin/ ; \
	rm docker.tgz; \
	dockerd --version; \
	docker --version




RUN command_1 && \
    command_2 && \
    ...
    command_n-2 && \
    command_n-1 && \
    command_n && \
    ...
    

["command_n-2", "command_n-1", " command_n "]


["DOCKER-FILE", "DOCKER-RUN", "BASH-AND", "BASH-COMMAND", "APT-GET-UPDATE"]
["DOCKER-FILE", "DOCKER-RUN", "BASH-AND", "BASH-COMMAND", "APT-GET-INSTALL", "FLAG-QUIET", "2"]
["DOCKER-FILE", "DOCKER-RUN", "BASH-AND", "BASH-COMMAND", "APT-GET-INSTALL", "PACKAGES", "PACKAGE", "..."]


["DOCKER-FILE", "DOCKER-RUN", "BASH-AND", "BASH-COMMAND", "APT-GET-UPDATE", "BASH-COMMAND", ...
 ... "DOCKER-RUN", "DOCKER-FILE", "DOCKER-RUN", ... "APT-GET-INSTALL", "FLAG-QUIET", "2", ... ]


["DOCKER-FILE", "DOCKER-RUN", "BASH-AND", "BASH-COMMAND", "APT-GET-UPDATE", "BASH-COMMAND", ...
 ... "BASH-AND", "BASH-COMMANDS", "APT-GET-INSTALL", "FLAG-QUIET", "2", ... ]



RUN set -ex \
	\
	&& wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz" \
	&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
	&& export GNUPGHOME="$(mktemp -d)" \
	&& gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \
	&& gpg --batch --verify python.tar.xz.asc python.tar.xz \
	&& { command -v gpgconf > /dev/null && gpgconf --kill all || :; } \
	&& rm -rf "$GNUPGHOME" python.tar.xz.asc \
	&& mkdir -p /usr/src/python \
	&& tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz \
	&& rm python.tar.xz \
	\
	&& cd /usr/src/python \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-shared \
		--enable-unicode=ucs4 \
	&& make -j "$(nproc)" \
	&& make install \
	&& ldconfig \
	\
	&& find /usr/local -depth \
		\( \
			\( -type d -a \( -name test -o -name tests \) \) \
			-o \
			\( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
		\) -exec rm -rf '{}' + \
	&& rm -rf /usr/src/python \
	\
	&& python2 --version

&& wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc" \
&& export GNUPGHOME="$(mktemp -d)" \
&& gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" \



&& gpg --batch --verify python.tar.xz.asc python.tar.xz \


```
