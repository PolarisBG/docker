FROM ubuntu:22.04

RUN apt-get update -qq && \
    apt-get install -y wget lsb-release software-properties-common gnupg \
                       apt-transport-https ca-certificates

WORKDIR /llvm
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh
RUN ./llvm.sh 17 all

RUN apt-get update -qq && \
    apt-get install -y clang git build-essential

WORKDIR /odin-build

RUN git clone https://github.com/odin-lang/Odin.git . \
    # TODO(gonz): make this version/tag configurable
    && git checkout dev-2023-12 \
    && LLVM_CONFIG="/usr/bin/llvm-config-17" make release

# Set clang as our default c++ compiler using LLVM 17
ENV CXX="/usr/bin/clang++-17"

# Echo the version of clang we're using
RUN $CXX --version

RUN mkdir /odin-artifacts && cp -R ./odin ./core ./shared /odin-artifacts

WORKDIR /

RUN rm -rf /odin-build

ENV PATH="/odin-artifacts:${PATH}"
