# Stage 1: Build the Rust application from raw source code
FROM rust:1.75-slim AS builder

RUN apt-get update && apt-get install -y pkg-config libssl-dev git && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src
# Clone the repo directly inside the container to grab the root source files
RUN git clone https://github.com/brodiepeers/lxp-bridge.git app

WORKDIR /usr/src/app
RUN cargo build --release

# Stage 2: Bundle the compiled binary into the Home Assistant wrapper
FROM ghcr.io/home-assistant/amd64-base:latest

# Install runtime dependencies needed by run.sh (yq)
RUN apk add --no-cache yq

# Copy the fresh compiled binary from the builder stage
COPY --from=builder /usr/src/app/target/release/lxp-bridge /usr/local/bin/lxp-bridge

# Copy startup scripts from the local build context
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
