# syntax=docker/dockerfile:1.7
# Shared build toolchain for FabianBruengerOrg Rust/Leptos apps.
#
# Bakes in the expensive, rarely-changing parts of the app Dockerfiles
# (apt packages, Node.js, cargo-leptos, cargo-chef, the wasm32 target) so app
# builds only need `cargo chef cook` + `cargo leptos build` on top of this.
#
# Built and pushed by .github/workflows/build-rust-leptos-base.yml as
# ghcr.io/fabianbruengerorg/rust-leptos-base:rust<RUST_VERSION>-leptos<CARGO_LEPTOS_VERSION>

ARG RUST_VERSION=1.98
FROM rust:${RUST_VERSION}-bookworm

ARG NODE_MAJOR=24
ARG CARGO_LEPTOS_VERSION=0.3.2
ARG CARGO_CHEF_VERSION=0.1.73

# Build dependencies for Rust + TLS + frontend toolchain.
RUN apt-get update && apt-get install -y \
    pkg-config \
    libssl-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Node.js for Tailwind builds.
RUN curl -fsSL "https://deb.nodesource.com/setup_${NODE_MAJOR}.x" | bash - \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

# Keep these aligned with each app's local toolchain to avoid generated output drift.
RUN cargo install cargo-leptos --version "${CARGO_LEPTOS_VERSION}" --locked \
    && cargo install cargo-chef --version "${CARGO_CHEF_VERSION}" --locked

RUN rustup target add wasm32-unknown-unknown

WORKDIR /app
