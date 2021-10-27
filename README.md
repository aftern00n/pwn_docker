# pwn_docker

[简体中文](README.md) | [English](README-en.md)

这个项目是参考[skysider/pwndocker](https://github.com/skysider/pwndocker)，该项目已经集成了很多辅助pwn解题的工具和glibc的各种版本，那为什么我还要在建一个pwn_docker，原因主要有如下两点：

- 本人比较喜欢搭配vim + ohmyzsh的工具来编写exp，因此加入vimplus和ohmyzsh
- skysider/pwndocker虽然集成了众多版本的glibc，但是无法进行源码调试



## 使用

pwn_docker镜像已经上传至docker hub，你可以直接下载。可使用docker exec进入容器，也可在容器中添加ssh公钥：

```
docker pull wuhouxiaoqi/pwn_docker
docker run --name ${container_name} -d -v ${ctf_path}:/ctf -p ${ssh_port}:22 --cap-add=SYS_PTRACE wuhouxiaoqi/pwn_docker
docker exec -it ${container_name} /bin/zsh
```

想要自己构建镜像的，先克隆本仓库：

```
git clone https://github.com/aftern00n/pwn_docker.git
```

然后构建glibc_docker镜像：

```
cd glibc_docker && docker build -t glibc_docker .
```

最后构建pwn_docker镜像：

```
cd pwn_docker && docker build -t pwn_docker .
```

注意构建pwn_docker之前，修改Dockerfile最后COPY --from指向的镜像名称为前面构建的gibc_docker镜像名称：

```
COPY --from=${glibc_docker_image_name} /glibc /glibc
```



## 已有软件

- [vimplus](https://github.com/chxuan/vimplus.git)
- [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh.git)
- [pwndbg](https://github.com/pwndbg/pwndbg.git)
- [Pwngdb](https://github.com/scwuaptx/Pwngdb.git)
- [pwntools](https://github.com/Gallopsled/pwntools.git)
-  [ropgadget](https://github.com/JonathanSalwan/ROPgadget.git)
- [one_gadget](https://github.com/david942j/one_gadget.git)
- [seccomp-tools](https://github.com/david942j/seccomp-tools.git)
- [z3-solver](https://github.com/Z3Prover/z3.git)
- [angr](https://github.com/angr/angr.git)
- [patchelf](https://github.com/NixOS/patchelf.git)
- [lief](https://github.com/lief-project/LIEF)
- [keystone](https://github.com/keystone-engine/keystone)
- [tmux](https://github.com/tmux/tmux.git)
- [ltrace](https://linux.die.net/man/1/ltrace)
- [strace](https://linux.die.net/man/1/strace)



## 已有glibc

源码和编译好的glibc默认都在/glibc目录：

```
➜  / tree -d /glibc -L 2
glibc
├── 32
│   ├── 2.27
│   ├── 2.28
│   ├── 2.29
│   ├── 2.30
│   ├── 2.31
│   ├── 2.33
│   └── 2.34
├── 64
│   ├── 2.23
│   ├── 2.24
│   ├── 2.26
│   ├── 2.27
│   ├── 2.28
│   ├── 2.29
│   ├── 2.30
│   ├── 2.31
│   ├── 2.33
│   └── 2.34
└── source
    ├── glibc-2.23
    ├── glibc-2.24
    ├── glibc-2.26
    ├── glibc-2.27
    ├── glibc-2.28
    ├── glibc-2.29
    ├── glibc-2.30
    ├── glibc-2.31
    ├── glibc-2.33
    └── glibc-2.34
```

pwn_docker是基于ubuntu18.04构建的，ubuntu18.04下glibc版本为2.27，如果想使用其他glibc版本运行程序，可以采取以下两种方案（首选第一种），以glibc-2.23为例：

```
patchelf --set-interpreter /glibc/64/2.23/lib/ld-2.23.so --set-rpath /glibc/64/2.23/lib binary
gdb ./binary
```

或

```
from pwn import *
p = process(["/glibc/64/2.23/lib/ld-2.23.so", "./binary"], env={"LD_PRELOAD":"/glibc/64/2.23/lib/libc.so.6"})
```



## 更新日志

### 2021-10-26

pwn_docker：增加env变量LANG=C.UTF-8，主要是为了解决pwntools的这个问题：[https://github.com/Gallopsled/pwntools/issues/1575](https://github.com/Gallopsled/pwntools/issues/1575)

glibc_docker：增加64位的glibc 2.33和2.34，以及从2.27至2.34的32位glibc。

### 2021-10-18

pwn_docker： python3添加z3-solver angr keystone-engine lief, python2添加z3-solver keystone-engine

### 2021-09-14

pwn_docker：给`.vimrc`添加ycm配置文件的全局变量。

glibc_docker：删除glibc源码编译过程中的build目录，不然镜像会比较大。
