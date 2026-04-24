# Hermes Agent Multi-Agent Initialization

Use `<profile>` for the personal agent profile or matching CLI alias. Replace `<user>` with the host user account where Hermes stores profile data.

## 1. Install Hermes Agent

```sh
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

## 2. Create an Agent Profile

```sh
hermes profile create <profile>
```

## 3. Configure the Agent Model and Channels

```sh
<profile> setup
```

## 4. Install the Gateway Autostart Service

```sh
<profile> gateway install
```

## 5. Start the Agent Gateway

```sh
<profile> gateway start
```

## 6. Configure the Memory Provider

```sh
<profile> memory setup
```

Interactive selections:

- Provider: `Hindsight`
- Mode: `Local External`

## 7. Optional: Configure the Hindsight Bank ID

Edit the Hindsight profile configuration if needed:

```sh
vim hindsight/config.json
```

## 8. Configure Browser Automation

```sh
<profile> tools
```

Interactive selections:

- `Reconfigure an existing tool's provider or API key`
- `Browser Automation`
- `Camofox`
- `http://localhost:9377`

## 9. Enable Camofox Managed Persistence

```sh
<profile> config set browser.camofox.managed_persistence true
```

## 10. Configure the Terminal Working Directory

```sh
<profile> config set terminal.cwd /home/<user>/.hermes/profiles/<profile>/home
```

## 11. Restart the Gateway

```sh
<profile> gateway restart
```

## 12. Interact With the Agent

After the gateway restarts, interact with the agent through the configured CLI or messaging channel.

## 13. Create Skills

Use the MiniMax CLI skill format as a reference:

```text
https://github.com/MiniMax-AI/cli/blob/main/skill/SKILL.md
```
