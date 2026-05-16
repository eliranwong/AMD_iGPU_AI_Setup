## Install Node.js

https://nodejs.org/en/download

## Install Linux Homebrew

https://brew.sh/

1. Execute the command line displayed at the top of the page.

2. Run the following script:

```
# set path
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"' >> ~/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
# opt out analytics
export HOMEBREW_NO_ANALYTICS=1
echo "export HOMEBREW_NO_ANALYTICS=1" >> ~/.bashrc
# Test
which brew
```

## Install Dependencies for OpenClaw and its Builtin SKILLs

```bash
npm install -g mcporter@latest
npm install -g clawhub@latest
npm install -g @steipete/summarize@latest
npm install -g @mariozechner/pi-ai@latest
```

## Install OpenClaw

```
npm i -g openclaw
```

## Install Ollama

Read https://ollama.com/, and run:

> curl -fsSL https://ollama.com/install.sh | sh

> ollama pull paraphrase-multilingual

# Activate Ollama Cloud

Subscribe to a cloud plan at https://ollama.com/pricing

```
ollama signin
ollama launch openclaw --config
```

Read more at https://docs.ollama.com/integrations/openclaw

# Enable OpenClaw to use Ollama web search

Copy the signature key from /usr/share/ollama/.ollama/id_ed25519 to ~/.ollama/

```
sudo cp /usr/share/ollama/.ollama/id_ed25519 ~/.ollama/
sudo chown $USER:$USER ~/.ollama/id_ed25519
chmod 600 ~/.ollama/id_ed25519
```

## Configure Discord

[Discord Setup](discord.md)

## Configure Coding Agents

[Coding Agents Setup](coding_agents.md)

## Configure and Run OpenClaw

Configure openclaw.json:

Take [openclaw.json](openclaw.json) as a reference and update it with your configuration.

Edit `~.openclaw/openclaw.json`, and run:

```
openclaw onboard --install-daemon
```

# Configure Web Search with SearXNG

## Install Docker Engine


Read https://docs.docker.com/engine/install/ubuntu/

Then, run:

```
sudo usermod -aG docker $LOGNAME
newgrp docker
```

## Install SearXNG

Read https://github.com/searxng/searxng

Recommendation: change the default 8080 to something else, e.g. 4000, in the .env file before run the install script.

## Configure OpenClaw Web Search to use SearXNG

```
openclaw configure
```

# Fix Progress Updates in Discord

Remove the following line from `~/.openclaw/openclaw.json` under the session `tools`:

```
"profile": "coding",
```

Read issue: https://github.com/openclaw/openclaw/issues/82747