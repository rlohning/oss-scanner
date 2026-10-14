# Network access is available during this Dockerfile build and is removed after this point, so
# all code that an agent might need to perform its tests must be fetched here.
FROM debian:bookworm
ENV DEBIAN_FRONTEND=noninteractive \
    CC=clang \
    CXX=clang++
RUN apt-get update && apt-get install -y --no-install-recommends \
      ca-certificates build-essential clang cmake libssl-dev ninja-build pkg-config git python3 \
    && rm -rf /var/lib/apt/lists/*

# scanner contract: the checkout lives inside the image, at /src
COPY . /src
RUN mkdir /src/build
WORKDIR /src/build

RUN /src/configure -opensource -confirm-license -prefix $PWD -release -force-debug-info -- \
                   -DCMAKE_C_FLAGS="-g -fno-omit-frame-pointer" \
    && cmake --build . -j"$(nproc)"

RUN cd /src/build && (ctest --output-on-failure -j"$(nproc)" --timeout 300 || echo "WARNING: ctest reported failures")
