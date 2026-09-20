# remotop

`remotop` is a btop-style terminal monitor for inbound SSH and remote shell sessions on Linux and macOS.

It shows live remote sessions, client address, login age, TTY, matching TCP socket details, and the visible process/activity tree for each session.
The detail pane also shows whether the client is local or remote, public-IP country, and best-effort client OS.
Suspicious sessions can be terminated from inside the app with an explicit confirmation.

## Status

This is an early MVP. It intentionally does **not** record keystrokes or terminal contents by default. The default view uses normal system accounting and process/socket metadata.

## Install with Homebrew

```bash
brew tap ricardusmd/remotop
brew install remotop
```

## Install on Ubuntu with APT

```bash
echo "deb [trusted=yes arch=all] https://raw.githubusercontent.com/ricardusmd/remotop/43e11166a8b1be7feec1196c9441bce9d0b216fe/apt stable main" | sudo tee /etc/apt/sources.list.d/remotop.list
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
- `K` terminate selected session, then `y` to confirm

## What it can see

Default mode:

- Active login sessions from `who -u`
- Fallback SSH session inference from `sshd` processes
- Source host/IP when exposed by the OS login database
- TTY and login PID where available
- Locality: local, remote LAN, or remote public
- Country for public IP addresses
- Client OS when it can be honestly inferred
- Process tree and commands running under the session
- TCP state and receive/send queue where visible
- In-app termination for the selected visible process tree, subject to OS permissions

Limitations:

- Per-session byte counters are platform-specific and not always exposed without kernel tracing or packet accounting.
- Country lookup uses `ipwho.is` for public IP addresses; private/LAN and local IPs are never sent for lookup.
- SSH does not normally expose the client OS, so this field is usually `unknown` for remote clients unless a future optional fingerprinting module is enabled.
- macOS socket-to-process visibility is more limited without elevated privileges.
- Termination sends `SIGTERM` to visible session processes. Run with `sudo` when terminating another user's/root-owned SSH session.
- Full terminal content capture should be an explicit opt-in audit/recording feature, not a silent monitor.

## Homebrew Tap

The tap repository is `ricardusmd/homebrew-remotop`.
