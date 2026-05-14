# Coding Agents

## Prerequisites

- Node.js 22.x

## OpenAI Codex via Azure Foundry

1. Create a new directory called `.codex` in home directory:

> mkdir -p ~/.codex

2. Copy the file [codex/config.toml](codex/config.toml) to `~/.codex/config.toml`.

3. Edit the `base_url` in the file `~/.codex/config.toml`.

4. Export `AZURE_OPENAI_API_KEY` in `~/.bashrc`:

```bash
export AZURE_OPENAI_API_KEY='...'
```

For details,ollow the instructions at:

https://learn.microsoft.com/en-us/azure/ai-foundry/openai/how-to/codex?view=foundry-classic&tabs=npm

## Claude Code via Azure Foundry

Set the following environment variables (replace the placeholder values with your own):

```bash
export ANTHROPIC_AUTH_TOKEN=<your_api_key>
export ANTHROPIC_BASE_URL=https://<your_project_name>.services.ai.azure.com/anthropic/
export ANTHROPIC_FOUNDRY_API_KEY=<your_api_key>
export ANTHROPIC_FOUNDRY_BASE_URL=https://<your_project_name>.services.ai.azure.com/anthropic/
export ANTHROPIC_MODEL=opusplan
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-5
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-5
```

Install and run Claude Code:

```bash
curl -fsSL https://claude.ai/install.sh | bash
claude
```

## Gemini CLI via Vertex AI

Set the following environment variables (replace the placeholder values with your own):

```bash
export GOOGLE_CLOUD_PROJECT=<your_project_id>
export GOOGLE_CLOUD_LOCATION=us-central1
export GOOGLE_APPLICATION_CREDENTIALS='/mnt/shared/Documents/credentials_google_cloud.json'  # path to your service account JSON key
```

Install and run Gemini CLI:

```bash
npm install -g @google/gemini-cli
gemini
```

Select **Vertex AI** when prompted for the authentication method.

# Install Gemini Deep Research Extension

Run:

> gemini extensions install https://github.com/allenhutchison/gemini-cli-deep-research --auto-update