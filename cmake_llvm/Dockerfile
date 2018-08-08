FROM ubuntu:bionic

# System Enviroment
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH=$PATH:/bin/:/usr/bin/:/usr/sbin/

# Locale Enviroment
ENV LANG fr_FR.UTF-8
ENV LANGUAGE fr_FR:en
ENV LC_ALL fr_FR.UTF-8

# Clang Enviroment
ENV LLVM_VERSION=6.0.1
ENV CC=clang
ENV CXX=clang++

# External required files
ADD llvm-snapshot.gpg.key llvm-snapshot.gpg.key
ADD apt_requirements.txt apt_requirements.txt

# Apt packages
RUN echo 'deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-6.0 main' >> /etc/apt/sources.list && \
    apt-key add ./llvm-snapshot.gpg.key && \
    apt-get update && \
    apt-get install -y $(grep -vE "^\s*#" apt_requirements.txt  | tr "\n" " ") && \
# Update Locale
    locale-gen fr_FR.UTF-8 && \
# Install CMake
    wget https://cmake.org/files/v3.12.0/cmake-3.12.0-Linux-x86_64.sh -O cmake.sh && \
    chmod +x ./cmake.sh && \
    ./cmake.sh --skip-license && \
# Configure Clang
    update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/clang clang /usr/bin/clang-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/clang-format clang-format /usr/bin/clang-format-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/clang-tidy clang-tidy /usr/bin/clang-tidy-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/lldb lldb /usr/bin/lldb-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/lld lld /usr/bin/lld-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/llvm-profdata llvm-profdata /usr/bin/llvm-profdata-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/llvm-cov llvm-cov /usr/bin/llvm-cov-$LLVM_VERSION 100 && \
    update-alternatives --install /usr/bin/llvm-symbolizer llvm-symbolizer /usr/bin/llvm-symbolizer-$LLVM_VERSION 100 && \
# Cleanup
    rm -fv ./cmake.sh && \
    apt-get -y autoremove && apt-get -y autoclean
