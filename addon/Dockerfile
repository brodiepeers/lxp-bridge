# Stage 1: Build the Rust application using an Alpine-based compiler
FROM rust:1.75-alpine AS builder

# Install building tools required for compilation on Alpine
RUN apk add --no-cache musl-dev openssl-dev pkgconfig git

WORKDIR /usr/src
RUN git clone https://github.com/brodiepeers/lxp-bridge.git app

WORKDIR /usr/src/app
# Force Rust to build using the musl target for absolute portability
RUN RUSTFLAGS="-C target-feature=+crt-static" cargo build --release --target x86_64-unknown-linux-musl

# Stage 2: Bundle the compiled binary into the Home Assistant wrapper
FROM ghcr.io/home-assistant/amd64-base:latest

RUN apk add --no-cache yq

# Copy the static binary from the musl target build directory
COPY --from=builder /usr/src/app/target/x86_64-unknown-linux-musl/release/lxp-bridge /usr/local/bin/lxp-bridge

COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
