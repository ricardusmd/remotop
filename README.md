# remotop

`remotop` is a btop-style terminal monitor for inbound SSH and remote shell sessions on Linux and macOS.

It shows live remote sessions, client address, login age, TTY, matching TCP socket details, and the visible process/activity tree for each session.

## Status

This is an early MVP. It intentionally does **not** record keystrokes or terminal contents by default. The default view uses normal system accounting and process/socket metadata.

## Install with Homebrew

```bash
brew tap ricardusmd/remotop
brew install remotop
```

## Install on Ubuntu with APT

```bash
echo "deb [trusted=yes] https://raw.githubusercontent.com/ricardusmd/remotop/main/apt stable main" | sudo tee /etc/apt/sources.list.d/remotop.list
sudo apt update
sudo apt install remotop
```

## Install locally as `remotop`

No virtualenv is required for normal local use:

```bash
make install
remotop
```

This creates `~/.local/bin/remotop`. If `~/.local/bin` is not in your shell `PATH`, add it:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

To make `sudo remotop` work on systems where `sudo` uses a restricted PATH:

```bash
make install-system
```

That creates `/usr/local/bin/remotop`.

## Install in editable Python mode

```bash
python3 -m pip install -e .
```

## Run

```bash
remotop
```

For better process and socket visibility:

```bash
sudo remotop
```

Options:

```bash
remotop --interval 0.5
remotop --no-resolve
```

Keys:

- `q` quit
- `up/down` or `k/j` select session
- `r` refresh now
- `p` or `space` pause

## What it can see

Default mode:

- Active login sessions from `who -u`
- Fallback SSH session inference from `sshd` processes
- Source host/IP when exposed by the OS login database
- TTY and login PID where available
- Process tree and commands running under the session
- TCP state and receive/send queue where visible

Limitations:

- Per-session byte counters are platform-specific and not always exposed without kernel tracing or packet accounting.
- macOS socket-to-process visibility is more limited without elevated privileges.
- Full terminal content capture should be an explicit opt-in audit/recording feature, not a silent monitor.

## Homebrew Tap

The tap repository is `ricardusmd/homebrew-remotop`.
