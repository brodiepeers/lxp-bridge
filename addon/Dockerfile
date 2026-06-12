# Stage 1: Build the Rust application from raw source code
FROM rust:1.75-slim AS builder

RUN apt-get update && apt-get install -y pkg-config libssl-dev git && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/lxp-bridge
COPY . .

RUN cargo build --release

# Stage 2: Bundle the compiled binary into the Home Assistant wrapper
FROM ghcr.io/home-assistant/amd64-base:latest

# Copy the fresh compiled binary from the builder stage
COPY --from=builder /usr/src/lxp-bridge/target/release/lxp-bridge /usr/bin/lxp-bridge

# Copy any required startup scripts/configs from your repository root
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
