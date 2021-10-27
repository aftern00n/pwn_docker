# pwn_docker

[简体中文](README.md) | [English](README-en.md)

This is a docker environment for pwn in ctf game. I reference a project called [skysider/pwndocker](https://github.com/skysider/pwndocker), which offers a docker including many useful tools used in pwn solving and different version of glibc. Since we have  skysider/pwndocker, why I still spend my energy to create a new docker? Because:

- I prefer using vim to write exp and ohmyzsh to configure terminal shell, so I add these two tools.
- Although skysider/pwndocker integrates many versions of glibc,  but it doesn't have glibc sources and I want to debug glibc in source level.

## Usage

The pwn_docker has been pushed into docker hub so you can docker pull it directly. You can control the docker by docker exec command or add ssh public key.

Run command:

```
docker pull wuhouxiaoqi/pwn_docker
docker run --name ${container_name} -d -v ${ctf_path}:/ctf -p ${ssh_port}:22 --cap-add=SYS_PTRACE wuhouxiaoqi/pwn_docker
docker exec -it ${container_name} /bin/zsh
```

If you want to build docker image for youself, firstly you should clone this project:

```
git clone https://github.com/aftern00n/pwn_docker.git
```

Then build the glibc_docker image:

```
cd glibc_docker && docker build -t glibc_docker .
```

And build the pwn_docker image:

```
cd pwn_docker && docker build -t pwn_docker .
```

PS: before building pwn_docker, you should update glibc_docker image name in Dockerfile of pwn_docker:

```
COPY --from=${glibc_docker_image_name} /glibc /glibc
```



## Installed software

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



## Installed glibc

The sources of glibc and compiled binarys are in the directory /glibc：

```
➜  / tree -d glibc -L 2
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

pwn_docker is  based on the image of ubuntu:18.04 whose glibc version is 2.27. If you want to run programs with other libc version, you can use the following two methods (the first is recommended), we take glibc 2.23 as an example:

```
patchelf --set-interpreter /glibc/64/2.23/lib/ld-2.23.so --set-rpath /glibc/64/2.23/lib binary
gdb ./binary
```

or

```
from pwn import *
p = process(["/glibc/64/2.23/lib/ld-2.23.so", "./binary"], env={"LD_PRELOAD":"/glibc/64/2.23/lib/libc.so.6"})
```



## Change Log

### 2021-10-26

pwn_docker: add env LANG=C.UTF-8，in order to solve this bug: [https://github.com/Gallopsled/pwntools/issues/1575](https://github.com/Gallopsled/pwntools/issues/1575)

glibc_docker: add 64 bit glibc 2.33 and 2.34, and 32 bit glibc from 2.27 to 2.34

### 2021-10-18

pwn_docker: python3 add z3-solver angr keystone-engine lief, python2 add z3-solver keystone-engine 

### 2021-09-14

pwn_docker: add `ycm_global_ycm_extra_conf` variable in `.vimrc`.

glibc_docker: delete the build folder after finishing compilation, otherwise the size of image would be too large.
