# Adds Chromium system libraries to nesquena/hermes-webui so Playwright-based
# browser tools work in-process inside the WebUI container.
#
# The Chromium *binary* itself comes from the gateway image via the shared
# `hermes-agent-src` named volume (mounted at /home/hermeswebui/.hermes/hermes-agent),
# which contains /opt/hermes/.playwright from the nousresearch/hermes-agent image.
# We only need the runtime .so dependencies here.
FROM ghcr.io/nesquena/hermes-webui:latest

USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
        libnss3 \
        libnspr4 \
        libatk1.0-0 \
        libatk-bridge2.0-0 \
        libcups2 \
        libxkbcommon0 \
        libxcomposite1 \
        libxdamage1 \
        libxrandr2 \
        libxfixes3 \
        libgbm1 \
        libpango-1.0-0 \
        libcairo2 \
        libasound2 \
        libatspi2.0-0 \
        libxshmfence1 \
        fonts-liberation \
    && rm -rf /var/lib/apt/lists/*

# Ensure a passwd/group entry exists for uid/gid 10000 so the WebUI can run as
# that uid (matching the gateway image's `hermes` user, which owns the shared
# /home/hermeswebui dir and the Playwright binaries inside the named volume).
# Without this, `whoami` and Python's pwd.getpwuid() fail and the entrypoint
# hangs. We use a neutral name (`hermesvol`) to avoid colliding with the
# upstream image's existing `hermeswebui`/`hermeswebuitoo` users.
RUN set -eux; \
    if ! getent group 10000 >/dev/null; then groupadd -g 10000 hermesvol; fi; \
    if ! getent passwd 10000 >/dev/null; then \
        useradd -u 10000 -g 10000 -d /home/hermeswebui -M -s /bin/bash hermesvol; \
    fi

USER hermeswebuitoo
