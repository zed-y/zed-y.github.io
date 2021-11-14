
# Docker的使用与基本操作

## Dokcer 安装环境

- Linux Ubuntu version 20.04
- Docker 19.03v.10

## Docker 安装

1. 安装 apt 依赖包，用于通过HTTPS来获取仓库:

        $ sudo apt-get install \
            apt-transport-https \
            ca-certificates \
            curl \
            gnupg-agent \
            software-properties-common
2. 添加 Docker 的官方 GPG 密钥：

        $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

3. 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88 通过搜索指纹的后8个字符，验证您现在是否拥有带有指纹的密钥。

        $ sudo apt-key fingerprint 0EBFCD88
        
        pub   rsa4096 2017-02-22 [SCEA]
            9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
        uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
        sub   rsa4096 2017-02-22 [S]

4. 使用以下指令设置稳定版仓库

        $ sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"

5. 安装最新版本的 Docker Engine-Community 和 containerd ：

        $ sudo apt-get install docker-ce docker-ce-cli containerd.io

6. 测试 Docker 是否安装成功，输入以下指令，打印出以下信息则安装成功:

        $ sudo docker run hello-world

        Unable to find image 'hello-world:latest' locally
        latest: Pulling from library/hello-world
        1b930d010525: Pull complete                                                                                                                                  Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
        Status: Downloaded newer image for hello-world:latest


        Hello from Docker!
        This message shows that your installation appears to be working correctly.


        To generate this message, Docker took the following steps:
        1. The Docker client contacted the Docker daemon.
        2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
            (amd64)
        3. The Docker daemon created a new container from that image which runs the
            executable that produces the output you are currently reading.
        4. The Docker daemon streamed that output to the Docker client, which sent it
            to your terminal.


        To try something more ambitious, you can run an Ubuntu container with:
        $ docker run -it ubuntu bash


        Share images, automate workflows, and more with a free Docker ID:
        https://hub.docker.com/


        For more examples and ideas, visit:
        https://docs.docker.com/get-started/


Docker安装完成后不能run hello-world，报错：docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled 的解决办法：
修改Docker源：

    修改或新增 /etc/docker/daemon.json

    # vi /etc/docker/daemon.json

    {

    "registry-mirrors": ["http://hub-mirror.c.163.com"]

    }
    //保存后退出在执行下面语句
    # systemctl restart docker.service

7. 参考链接

[https://www.runoob.com/docker/ubuntu-docker-install.html](https://www.runoob.com/docker/ubuntu-docker-install.html)

[https://blog.csdn.net/BigData_Mining/article/details/87869147](https://blog.csdn.net/BigData_Mining/article/details/87869147)

## Docker 使用

在使用之前，为保证镜像下载速度，可以更换镜像源：

[Docker 修改镜像源地址](https://www.jianshu.com/p/df75f9b5fcf6)

Docker的基础使用语句有以下几种：拉取镜像，上传镜像，运行容器，提交镜像，保存为tar文件，加载tar文件，Dockerfile来build镜像

#### 拉取镜像

Docker可以使用pull命令来拉去远程共有仓库中的镜像到本机使用，具体方法如下：

        root$ docker search tomcat

首先利用search来查找要获取的镜像名字

![docker1](https://s1.ax1x.com/2020/06/05/tDT2wQ.md.png)

然后获取自己需要的镜像，以第一个的为例：

        root$ docker pull tomcat

默认版本为latest，如果需要特定版本，可以加:版本号，如tomcat:8

![docker2](https://s1.ax1x.com/2020/06/05/tDTgeg.md.png)

下载完成后，便可以查看本地仓库的镜像

    root$ docker image ls

![docker3](https://s1.ax1x.com/2020/06/05/tDTsQf.md.png)

#### 上传镜像

当对一个官方镜像做了一个修改，成为自己的镜像后，可以选择上传到DokcerHub上，作为一个自己的独特的镜像，此处略去过程。

#### 运行容器

通过docker run命令来运行一个容器，例如下面这个例子。

    root$ docker run --name Mytom -d -p 8888:8080 tomcat

上述命令中，--name为指定运行的容器的名字，方便以后管理，-d为保持后台运行，因为tomcat已启动后，便会占用此窗口来显示其日志，-p为映射端口，: 之前的端口为宿主机的端口，: 之后的端口为容器内的端口，因为容器必须要映射到主机之后才可以访问。

关于dockers run的具体参数不与解释，详情见[网络资料](https://www.runoob.com/docker/docker-run-command.html)

运行后通过docker container ps 命令来查看当前运行容器情况，加上-a参数表示查看所有容器，包括已经停止的。

![docker4](https://s1.ax1x.com/2020/06/05/tDT6OS.md.png)

则会使通过浏览器访问8888端口便会发现tomcat启动成功。

![docker6](https://s1.ax1x.com/2020/06/05/tDTfFs.md.png)

PS: 如果发现浏览器打开后报404错误，见[解决方法](https://blog.csdn.net/qq_40891009/article/details/103898876)

##### 进入容器

通过命令 docker exec -it 唯一标识 /bin/bash 来进入到容器的SHELL中进行操作,对容器内部来进行操作。

其中 唯一标识可以是容器的ID，也可以是名字，-it表示容器的STDIN(标准输入输出)处于打开状态，同时分配给其一个伪终端。

![docker5](https://s1.ax1x.com/2020/06/05/tDTyy8.md.png)

#### 提交镜像

该命令类比于push命令，只不过是将镜像只提交到本地的私有仓库中，一旦删除，便无法下载。

如下：

    # root$ docker commit 容器的id 镜像的名字:版本号
    root$ docker commit 9f951bcb20d7 yzd/mytomcat:v1

上命令便表示将9f容器提交到yzd/mytomcat镜像，版本号为v1。

commit具体参数见[https://www.runoob.com/docker/docker-commit-command.html](https://www.runoob.com/docker/docker-commit-command.html)

当再次查看镜像时便会发现多了一个镜像

![docker7](https://s1.ax1x.com/2020/06/05/tDTRoj.md.png)

#### 保存与加载tar文件

保存命令：docker save -o 输出文件 镜像名字:版本号

![docker8](https://s1.ax1x.com/2020/06/05/tDThYn.md.png)

加载命令：docker load < tar文件

![docker9](https://s1.ax1x.com/2020/06/05/tDT4Wq.md.png)

#### 重头戏--Dockerfile来build镜像

指令：

    - FROM 定制的镜像都是基于 FROM 的镜像
    - COPY 从宿主机目录中复制文件或者目录到容器里指定路径
    - ADD 与COPY命令相似，只不过ADD可以解压压缩文件，还可以从URL下载网络资源，而COPY不可以
    - WORKDIR 指定容器工作目录。用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在。（WORKDIR 指定的工作目录，必须是提前创建好的）。
    - RUN 在docker build 时执行后面的命令
    - CMD 在docker run 时运行执行后面的命令
    - ENTYRYPOINT 允许您配置将作为可执行文件运行的容器。它看起来类似于CMD，因为它还允许您使用参数指定命令。区别在于当Docker容器使用命令行参数运行时，ENTRYPOINT命令不会忽略参数。
![docker10](https://s1.ax1x.com/2020/06/05/tDTIS0.md.png)

    - EXPOSE 声明镜像暴露出来的端口
    - VOLUME 定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。(不常用)
    - ENV 设置环境变量，定义了环境变量，那么在后续的指令中，就可以使用这个环境变量。
    - ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效。
    - ONBUILD 用于延迟构建命令的执行。简单的说，就是 Dockerfile 里用 ONBUILD 指定的命令，在本次构建镜像的过程中不会执行（假设镜像为 test-build）。当有新的 Dockerfile 使用了之前构建的镜像 FROM test-build ，这是执行新镜像的 Dockerfile 构建时候，会执行 test-build 的 Dockerfile 里的 ONBUILD 指定的命令。
Dockersfile具体指令可以见[菜鸟教程](https://www.runoob.com/docker/docker-dockerfile.html)或[官方文档](https://docs.docker.com/engine/reference/builder/)

一个例子：

![docker11](https://s1.ax1x.com/2020/06/05/tDTolV.md.png)

然后进行构建与运行

![docker12](https://s1.ax1x.com/2020/06/05/tDTTyT.md.png)

可以看到1.txt中的内容变为了2020，说明执行了RUN命令修改了其中的内容。