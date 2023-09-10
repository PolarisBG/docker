FROM ubuntu:20.04

RUN apt-get update -qq && \
    apt-get install -y wget lsb-release software-properties-common gnupg \
                       apt-transport-https ca-certificates

WORKDIR /llvm
RUN wget https://apt.llvm.org/llvm.sh && chmod +x llvm.sh
RUN ./llvm.sh 14

RUN apt-get update -qq && \
    apt-get install -y clang git build-essential

WORKDIR /odin-build

RUN git clone https://github.com/odin-lang/Odin.git . \
    # TODO(gonz): make this version/tag configurable
    && git checkout dev-2023-08 \
    && LLVM_CONFIG="/usr/bin/llvm-config-14" make release

RUN mkdir /odin-artifacts && cp -R ./odin ./core ./shared /odin-artifacts

WORKDIR /

RUN rm -rf /odin-build

ENV PATH="/odin-artifacts:${PATH}"
