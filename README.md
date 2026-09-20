# remotop

`remotop` is a btop-style terminal monitor for inbound SSH and remote shell sessions on Linux and macOS.

It shows live remote sessions, client address, login age, TTY, matching TCP socket details, and the visible process/activity tree for each session.

## Status

This is an early MVP. It intentionally does **not** record keystrokes or terminal contents by default. The default view uses normal system accounting and process/socket metadata.

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

## Packaging Roadmap

Homebrew tap layout is included in `Formula/remotop.rb`.

Planned release steps:

1. Publish `ricardusmd/remotop`.
2. Tag `v0.1.0`.
3. Generate the release tarball SHA256.
4. Publish `ricardusmd/homebrew-remotop` with `Formula/remotop.rb`.
5. Install with:

```bash
brew tap ricardusmd/remotop
brew install remotop
```
