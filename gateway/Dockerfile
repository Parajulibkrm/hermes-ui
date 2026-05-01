# Adds full Chromium to nousresearch/hermes-agent for browser tools
# (browser_navigate etc.). The base image only ships chromium_headless_shell,
# which Playwright uses only when headless="new" is requested explicitly. Most
# hermes browser tool calls launch full chromium and otherwise fail with
# "Executable doesn't exist at .../chromium-XXXX/...".
ARG HERMES_IMAGE=nousresearch/hermes-agent:latest
FROM ${HERMES_IMAGE}

USER hermes
RUN PLAYWRIGHT_BROWSERS_PATH=/opt/hermes/.playwright \
    /opt/hermes/node_modules/.bin/playwright install chromium
