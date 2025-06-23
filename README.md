# ChatGPT Codex JSON-Driven Setup Script Runner

A single-file Bash + JSON setup runner built specifically for ChatGPT Codex's environment setup interface. Created to streamline environment provisioning by reducing output noise—displaying only section headers by default and revealing full details only when a step fails.

## What is ChatGPT Codex?

ChatGPT Codex is OpenAI's cloud-based development environment that provides ephemeral Ubuntu 24.04 containers for coding projects. Each environment is linked one-to-one with a GitHub repository, allowing you to develop, test, and deploy code in a fully configured cloud instance without local setup requirements.

### Key Features of ChatGPT Codex:
- **Ephemeral Ubuntu 24.04 containers** - Fresh, isolated environments for each project
- **GitHub integration** - Direct repository linking and synchronization
- **Pre-installed frameworks** - Comes with Python, Node.js, and many popular development frameworks
- **Environment variables & secrets management** - Secure configuration for API keys and sensitive data
- **Custom setup scripts** - Automated environment provisioning on container creation

## How to Access ChatGPT Codex

1. **Direct URL**: Navigate to https://chatgpt.com/codex
2. **From ChatGPT**: Click the "Codex" link in the left sidebar (next to Sora and GPTs)

## Setting Up a New Environment

1. **Navigate to Environments**: In Codex, click "Environments" in the top-right area
2. **Create Environment**: Click the "+ Create Environment" button
3. **Link GitHub Repository**: Connect your project repository (one-to-one relationship)
4. **Configure Variables**: Set environment variables and secrets in the interface above the setup script area
5. **Add Setup Script**: Paste your setup script in the textarea (this is where our tool comes in!)

## Why Use This Setup Script Runner?

Traditional setup scripts can produce overwhelming output, making it difficult to track progress and identify issues. This tool addresses that by:

- **Reducing noise**: Shows only section headers during execution
- **Highlighting failures**: Displays full output only when errors occur
- **Organized structure**: JSON-driven configuration keeps setup organized
- **Immediate feedback**: Script halts on first failure with detailed error information

This is particularly valuable for Laravel, Node.js, Python, and other complex environments that require multiple installation and configuration steps.

## Overview

This tool lets you define both the environment variables you require and the sections of commands you want to run—all in a single file. It validates required variables, runs sections either sequentially or in parallel, and prints only section headers by default. If a section fails, you immediately see its full output and the script exits.

## Features

* **Embedded JSON configuration**: Declare `variables` and `sections` at the top of the script.
* **Environment-variable validation**: Fails early if any required variable from `variables` isn't set.
* **Sequential & parallel execution**: Per-section `parallel: true` flag for concurrent installs.
* **Error handling & logging**: Shows concise logs, prints full output on failure, and halts immediately.
* **Dry-run support**: Simply set `DRY_RUN=true` in your environment to preview commands without executing them.
* **Single-file simplicity**: All configuration and logic lives in one Bash script—no external dependencies other than `jq`.

## Requirements

* **ChatGPT Codex environment**: This script is designed and tested to work seamlessly within the Codex setup script interface.
* **jq**: JSON processor (typically pre-installed in Codex environments)

## Setup Instructions

### Step 1: Prepare Your Environment Variables
1. In your Codex environment setup, locate the "Environment Variables & Secrets" section (directly above the setup script textarea)
2. Add any required variables (e.g., `MANDRILL_API_KEY`, `DATABASE_PASSWORD`)
3. Use "Environment Variables" for non-sensitive data and "Secrets" for sensitive information like API keys

### Step 2: Configure the Setup Script
1. Copy the entire contents of `setup.sh` (or the script template) into the Codex setup script textarea
2. In the JSON at the top of the script, list any environment variables your setup requires under the `"variables"` array
3. Define your setup steps by adding entries under `"sections"`, each with:
   * `"desc"`: a short description of what this section does
   * `"cmds"`: an array of commands to run
   * Optional `"parallel": true` to run commands in that section concurrently

### Step 3: Test with Dry Run (Optional)
Set `DRY_RUN=true` in your environment variables to preview all commands without executing them.

## ChatGPT Codex Custom Instructions (Optional)

This step is **optional** and only needed if you want ChatGPT to be able to intelligently modify the setup script for you. Even with these instructions, you'll still need to manually copy the contents from the `script.js` file in your repository and paste it into the Environment setup script textarea.

### Setup Instructions:
1. Navigate to **Codex Settings → General → Custom instructions**
2. Add the snippet below to enable ChatGPT to generate or modify setup scripts for you
3. **Important**: You must still manually copy the actual script content from your repository's `script.js` file into the Codex environment setup textarea

### Custom Instructions Snippet:
```
# Setup Script
filename: script.js

When asked to create or edit the setup script, this is for the ChatGPT Codex environment setup script. It is for provisioning the server used for Codex. You can find the template here: https://raw.githubusercontent.com/bryanschmidty/openai-codex-setupscript/refs/heads/main/setup
you can find instructions on how to use it, and an example JSON structure here: https://raw.githubusercontent.com/bryanschmidty/openai-codex-setupscript/refs/heads/main/README.md
```

### Customizing the Filename:
If `script.js` conflicts with an actual file in your repository, or if you prefer a different name or location, you can modify the custom instructions accordingly. For example:
- `filename: setup-script.js`
- `filename: scripts/codex-setup.js`
- `filename: .codex/setup.sh`

Simply update the `filename:` line in your custom instructions to match your preferred file structure.

## JSON Structure Example

Here's a practical example for setting up a Laravel environment:

```json
{
  "variables": [
    "APP_KEY",
    "DATABASE_PASSWORD",
    "MAIL_PASSWORD"
  ],
  "sections": [
    {
      "desc": "Update system packages",
      "cmds": ["sudo apt-get update -y", "sudo apt-get upgrade -y"]
    },
    {
      "desc": "Install PHP and extensions",
      "parallel": true,
      "cmds": [
        "sudo apt-get install -y php8.3-cli php8.3-fpm",
        "sudo apt-get install -y php8.3-mysql php8.3-xml php8.3-curl"
      ]
    },
    {
      "desc": "Install Composer",
      "cmds": [
        "curl -sS https://getcomposer.org/installer | php",
        "sudo mv composer.phar /usr/local/bin/composer"
      ]
    },
    {
      "desc": "Setup Laravel project",
      "cmds": [
        "composer install --no-dev --optimize-autoloader",
        "php artisan key:generate",
        "php artisan migrate --force"
      ]
    }
  ]
}
```

## Common Use Cases

- **Laravel applications**: PHP, Composer, database setup, and Laravel-specific configurations
- **Node.js projects**: npm/yarn installs, PM2 setup, environment configuration
- **Python applications**: pip installs, virtual environments, Django/Flask setup
- **Full-stack applications**: Multiple language runtimes, databases, and service configurations
- **DevOps tooling**: Docker, monitoring agents, deployment scripts

## Dry Run Mode

To preview all commands without executing them:
1. Add `DRY_RUN` as an environment variable in your Codex environment
2. Set its value to `true`
3. The script will display all commands it would run without making any changes

This is particularly useful for:
- Debugging setup scripts
- Reviewing commands before execution
- Training and documentation purposes

## To-Do

* Add per-section timeouts
* Option to output the stdout of a specific section
* Enable timing output (elapsed time per section and total time)
* Keep compatibility updated as ChatGPT Codex evolves
* Add more framework-specific examples

## Contributing

Contributions are welcome! Please fork the repo, make your changes, and open a pull request. If you find bugs or have feature requests, [open an issue](https://github.com/bryanschmidty/openai-codex-setupscript/issues).

## Donations

If you find this tool helpful, please consider donating:

[![PayPal Donate](https://img.shields.io/badge/PayPal-Donate-gold?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=9M7BNKVF9TYF8)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-gold?logo=buymeacoffee)](https://coff.ee/bryanschmidt)
