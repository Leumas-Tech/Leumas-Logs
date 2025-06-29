# Leumas Activity Logger

A background Node.js service that tracks your daily file activity and automatically commits and pushes logs to GitHub. Perfect for developers who code regularly but forget to push their work — now your GitHub "grass" stays green!

## Motivation

* **Never miss a commit again**: Even if you don’t push code frequently, this tool records every file create, edit, and delete you make.
* **Grow your GitHub grass**: Keep your GitHub contributions graph active by logging your work as daily timeline entries.
* **Zero friction**: Runs silently in the background alongside your other tools, automatically handling commits and pushes.

## Features

* **Instant file watching**: Uses polling-based `chokidar` to detect changes in real time.
* **Daily logs**: Appends all events to `logs/YYYY-MM-DD.txt` files.
* **Auto Git setup**: Initializes a Git repo in `./logs`, sets the branch, and configures your remote.
* **Scheduled commits & pushes**: Commits changes locally and force-pushes to GitHub at your defined interval.
* **Human-readable entries**: Timestamps in `YYYY-MM-DD HH:MM:SS` format with descriptive actions.
* **Configurable**: Customize watch path, interval, branch, and remote via `.env`.

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/Leumas-Tech/Leumas-Activity-Logger.git
   cd Leumas-Activity-Logger
   ```
2. **Install dependencies**

   ```bash
   npm install
   ```

## Configuration

Create a `.env` file in the project root with these variables:

```dotenv
# Directory to watch for activity
WATCH_PATH=D:/Leumas/Servers/x.water-grass

# Port for the HTTP health-check server (optional)
PORT=3000

# Minutes between each automatic commit & push\ nCOMMIT_INTERVAL_MINUTES=60

# (Optional) Custom path for the Git logs repo; defaults to ./logs
LOG_REPO_PATH=

# Your GitHub repo URL (SSH or HTTPS)
GIT_REMOTE_URL=git@github.com:Leumas-Tech/Leumas-Logs.git

# Branch name on the remote (default: main)
GIT_REMOTE_BRANCH=main

# (Optional) GitHub personal access token to avoid SSH issues
GITHUB_TOKEN=
```

> **Note**: If using a GitHub token, it will be embedded in the HTTPS remote URL to authenticate pushes over HTTPS.

## Usage

Start the logger as part of your startup scripts or manually:

```bash
node server.js
```

* **Health check**: Visit `http://localhost:3000/` to confirm the service is running.
* **Log files**: Check the `logs/` directory for daily `YYYY-MM-DD.txt` files.
* **Console output**: Watch real-time events and a countdown to the next push.

## How It Works

1. **Startup**: Reads configuration from `.env` and initializes (or reinitializes) the Git repo in `LOG_REPO_PATH`.
2. **File watching**: Uses `chokidar` in polling mode for reliable, immediate detection of file changes.
3. **Logging**: Appends each event (create/edit/delete) to the current day’s log file with a human-friendly timestamp.
4. **Commit & push**: Every `COMMIT_INTERVAL_MINUTES`, stages all log files, commits with a timestamped message, and force-pushes to the remote GitHub repo.

## Customization

* **Event mapping**: Modify the `HUMAN_ACTIONS` map in `server.js` for different descriptions.
* **Polling intervals**: Adjust `chokidar`’s `usePolling`, `interval`, and `awaitWriteFinish` options for performance.
* **Commit behavior**: Change or remove `--force` in the push command for safer merges.

## Troubleshooting

* **No logs in GitHub**: Ensure `GIT_REMOTE_URL` is correct, and your GitHub token (if used) has `repo` permissions.
* **Permission errors**: Use an HTTPS remote with `GITHUB_TOKEN`, or configure SSH keys properly.
* **Excessive commits**: Increase `COMMIT_INTERVAL_MINUTES` to reduce frequency.

## Contributing

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/...`)
3. Commit your changes
4. Open a pull request

## License

MIT © Leumas Technologies
