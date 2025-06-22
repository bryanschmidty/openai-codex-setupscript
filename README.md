# OpenAI Codex JSON-Driven Setup Script Runner

A single-file Bash + JSON setup runner built specifically for OpenAI Codex’s single-textarea environment.
Created to mute the noise of traditional setup scripts—displaying only section headers by default and revealing full details only when a step fails.

## Overview

This tool lets you define both the environment variables you require and the sections of commands you want to run—all in a single file. It validates required variables, runs sections either sequentially or in parallel, and prints only section headers by default. If a section fails, you immediately see its full output and the script exits.

## Features

* **Embedded JSON configuration**: Declare `variables` and `sections` at the top of the script.
* **Environment-variable validation**: Fails early if any required variable from `variables` isn’t set.
* **Sequential & parallel execution**: Per-section `parallel: true` flag for concurrent installs.
* **Error handling & logging**: Shows concise logs, prints full output on failure, and halts immediately.
* **Dry-run support**: Simply set `DRY_RUN=true` in your environment to preview commands without executing them.
* **Single-file simplicity**: All configuration and logic lives in one Bash script—no external dependencies other than `jq`.

## Requirements

* **OpenAI Codex environment**: this script is designed and tested to work seamlessly within the Codex single-textarea interface.

## Setup

1. Copy the entire contents of `setup.sh` (or the script template) into the Codex single-textarea input.
2. In the JSON at the top of the script, list any environment variables your setup requires under the `"variables"` array.
3. Use Codex’s Env & Secrets interface to set those variables (e.g. `MANDRILL_API_KEY`, `DATABASE_PASSWORD`, `DRY_RUN`).
4. Define your setup steps by adding entries under `"sections"`, each with:

   * `"desc"`: a short description
   * `"cmds"`: an array of commands to run
   * Optional `"parallel": true` to run that section concurrently

## Codex Custom Instructions

Add the snippet below to **Codex Settings → General → Custom instructions**. Once saved, you can request Codex to generate or modify the environment setup script directly within any project.

```
# Setup Script

When asked to create or edit the setup script, this is for the OpenAI Codex environment setup script. It is for provisioning the server used for Codex. You can find the template here: https://raw.githubusercontent.com/bryanschmidty/openai-codex-setupscript/refs/heads/main/setup
you can find instructions on how to use it, and an example JSON structure here: https://raw.githubusercontent.com/bryanschmidty/openai-codex-setupscript/refs/heads/main/README.md
```

## Dry Run

To preview all commands without executing them, set the `DRY_RUN` environment variable to `"true"` in the Codex Env & Secrets interface. The script will display the commands it *would* run without making any changes.

## JSON Structure Example

```json
{
  "variables": [
    "MANDRILL_API_KEY",
    "DATABASE_PASSWORD"
  ],
  "sections": [
    {
      "desc": "Update packages",
      "cmds": ["sudo apt-get update -y"]
    },
    {
      "desc": "Install tools",
      "parallel": true,
      "cmds": ["sudo apt-get install -y curl", "sudo apt-get install -y jq"]
    }
  ]
}
```

## To-Do

* Add per-section timeouts
* Option to output the std out of a specific section
* Set timeout for a section
* Enable timing, which will output the elapsed time of each section and the total time
* Keep compatibility updated as OpenAI Codex evolves

## Contributing

Contributions are welcome! Please fork the repo, make your changes, and open a pull request. If you find bugs or have feature requests, [open an issue](https://github.com/yourusername/json-setup-runner/issues).

## Donations

If you find this tool helpful, please consider donating:

[![PayPal Donate](https://img.shields.io/badge/PayPal-Donate-gold?logo=paypal)](https://www.paypal.com/donate/?hosted_button_id=9M7BNKVF9TYF8)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-Donate-gold?logo=buymeacoffee)](https://coff.ee/bryanschmidt)
