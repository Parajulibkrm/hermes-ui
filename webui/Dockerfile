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

USER hermeswebuitoo
