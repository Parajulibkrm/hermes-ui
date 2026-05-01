# Adds full Chromium to nousresearch/hermes-agent for browser tools
# (browser_navigate etc.). The base image only ships chromium_headless_shell,
# which Playwright uses only when headless="new" is requested explicitly. Most
# hermes browser tool calls launch full chromium and otherwise fail with
# "Executable doesn't exist at .../chromium-XXXX/...".
ARG HERMES_IMAGE=nousresearch/hermes-agent:latest
FROM ${HERMES_IMAGE}

# Install as root: at build time /opt/hermes/.playwright is still root-owned
# (base image's entrypoint chowns it to hermes only at container start, which
# never runs during a build), so USER hermes hits EACCES on the lockfile.
# HOME is set so npm/playwright don't try to write into /root.
USER root
RUN HOME=/opt/hermes \
    PLAYWRIGHT_BROWSERS_PATH=/opt/hermes/.playwright \
    /opt/hermes/node_modules/.bin/playwright install chromium && \
    chown -R hermes:hermes /opt/hermes/.playwright
