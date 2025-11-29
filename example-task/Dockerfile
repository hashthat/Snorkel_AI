# syntax = docker/dockerfile:1
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=xterm-256color \
    LANG=C.UTF-8

# Install only the absolute minimum we need
RUN apt-get update && apt-get install -y --no-install-recommends \
    vim-tiny \
    python3 \
    python3-pip \
    diffutils \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Python test dependencies — works on every runner
RUN pip3 install --no-cache-dir --root-user-action=ignore pytest pyyaml


# Create the non-root user exactly as Terminal-Bench expects
RUN useradd -m -u 1001 -s /bin/bash agent

WORKDIR /workspace

# Copy data and tests at build time
COPY data/ /workspace/data/
COPY tests/ /workspace/tests/

# Make the expected file completely unreadable (prevents cheating)
RUN chmod 000 /workspace/data/expected.txt

# Everything else must be owned and writable by the agent
RUN chown -R agent:agent /workspace

# Switch to the agent user (required by the benchmark)
USER agent

# Default command — Terminal-Bench will override it anyway
CMD ["/bin/bash"]
