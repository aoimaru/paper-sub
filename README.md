






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
