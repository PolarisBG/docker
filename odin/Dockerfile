FROM ubuntu:14.04

RUN apt-get update -qq && apt-get install -y llvm-12 clang git build-essential

WORKDIR /odin-build

RUN git clone https://github.com/odin-lang/Odin.git . \
    # TODO(gonz): make this version/tag configurable
    && git checkout dev-2023-08 \
    && make release

RUN mkdir /odin-artifacts && cp -R ./odin ./core ./shared ./tools /odin-artifacts

WORKDIR /

RUN rm -rf /odin-build

ENV PATH="/odin-artifacts:${PATH}"
