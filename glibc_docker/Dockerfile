FROM ubuntu:bionic
RUN sed -i "s/http:\/\/archive.ubuntu.com/http:\/\/mirrors.tuna.tsinghua.edu.cn/g" /etc/apt/sources.list && \
    apt-get update && apt-get -y dist-upgrade && \
    apt-get install -y wget git vim && \
    apt-get install -y gcc cmake gawk bison python3

WORKDIR /root
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.23.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.24.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.26.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.27.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.29.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.30.tar.gz
RUN wget http://ftp.gnu.org/gnu/glibc/glibc-2.31.tar.gz

RUN mkdir -p /glibc/source
RUN tar -zxvf glibc-2.23.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.24.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.26.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.27.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.28.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.29.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.30.tar.gz -C /glibc/source
RUN tar -zxvf glibc-2.31.tar.gz -C /glibc/source

COPY ./2.23-patch /root/2.23-patch
COPY ./2.24-patch /root/2.24-patch

RUN cd /glibc/source/glibc-2.23 && find /root/2.23-patch/ -type f -name "*.patch" -exec patch -p1 -i {} \;
RUN cd /glibc/source/glibc-2.24 && find /root/2.24-patch/ -type f -name "*.patch" -exec patch -p1 -i {} \;

RUN mkdir -p /glibc/64
RUN cd /glibc/source/glibc-2.23 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.23 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.24 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.24 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.26 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.26 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.27 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.27 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.28 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.28 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.29 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.29 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.30 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.30 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN cd /glibc/source/glibc-2.31 && mkdir build && \
    cd build && ../configure --prefix=/glibc/64/2.31 --enable-debug=yes CFLAGS="-O2 -g" CPPFLAGS="-O2 -g" && \
    make -j `nproc` && make install -j `nproc`

RUN rm -rf /glibc/source/glibc-2.23/build && \
    rm -rf /glibc/source/glibc-2.24/build && \
    rm -rf /glibc/source/glibc-2.26/build && \
    rm -rf /glibc/source/glibc-2.27/build && \
    rm -rf /glibc/source/glibc-2.28/build && \
    rm -rf /glibc/source/glibc-2.29/build && \
    rm -rf /glibc/source/glibc-2.30/build && \
    rm -rf /glibc/source/glibc-2.31/build

ENTRYPOINT sleep infinity
