# pwn_docker

[简体中文](README.md) | [English](README-en.md)

This is a docker environment for pwn in ctf game. I reference a project called [skysider/pwndocker](https://github.com/skysider/pwndocker), which offers a docker including many useful tools used in pwn solving and different version of glibc. Since we have  skysider/pwndocker, Why I still spend my energy to create a new docker? because:

- I like using vim to write exp and ohmyzsh to configure terminal shell, so I add these two tools.
- Although skysider/pwndocker integrates many versions of glibc,  but it doesn't have glibc source code and I want to debug glibc in source level.

## Usage

The pwn_docker has been pushed into docker hub so you can docker pull it directly. You can control the docker by docker exec command or add ssh public key.

Run command:

```
docker pull wuhouxiaoqi/pwn_docker
docker run --name ${container_name} -d -v ${ctf_path}:/ctf -p ${ssh_port}:22 --cap-add=SYS_PTRACE wuhouxiaoqi/pwn_docker
docker exec -it ${container_name} /bin/zsh
```

If you want to build docker image for youself, firstly you should clont the project:

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
- [tmux](https://github.com/tmux/tmux.git)
- [ltrace](https://linux.die.net/man/1/ltrace)
- [strace](https://linux.die.net/man/1/strace)



## Installed glibc

The sources of glibc and compiled binarys are in the directory /glibc：

```
/glibc
|-- 64
|   |-- 2.23
|   |-- 2.24
|   |-- 2.26
|   |-- 2.27
|   |-- 2.28
|   |-- 2.29
|   |-- 2.30
|   `-- 2.31
`-- source
    |-- glibc-2.23
    |-- glibc-2.24
    |-- glibc-2.26
    |-- glibc-2.27
    |-- glibc-2.28
    |-- glibc-2.29
    |-- glibc-2.30
    `-- glibc-2.31
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
