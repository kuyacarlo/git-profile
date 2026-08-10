# Install: git-profile + git-ssh

One install path for the identity pair:

| Tool | Binary | Repo | Role |
| --- | --- | --- | --- |
| **git-profile** | `git-profile` | [kuyacarlo/git-profile](https://github.com/kuyacarlo/git-profile) (fork of [dotzero/git-profile](https://github.com/dotzero/git-profile)) | Per-repo Git identity (`user.name`, `user.email`, signing key, …) |
| **git-ssh** | `git-ssh` | [kuyacarlo/ssh-profile](https://github.com/kuyacarlo/ssh-profile) | Per-repo SSH key + origin URL (no `~/.ssh/config` Host aliases) |

Use the **same profile name** in both tools (for example `alice`). Markers in the repo:

- `current-profile.name` — set by `git-profile`
- `current-profile.ssh` — set by `git-ssh`

---

## Install git-profile

### Homebrew

```bash
brew install dotzero/tap/git-profile
```

### Prebuilt binaries

Download a binary from the [upstream releases](https://github.com/dotzero/git-profile/releases) page and place it on `$PATH`.

### Build from source

```bash
go install github.com/dotzero/git-profile@latest
```

The binary lands in `$GOBIN` or `$GOPATH/bin`. Confirm:

```bash
git-profile version
# or: command -v git-profile
```

---

## Install git-ssh (ssh-profile)

The CLI binary is named **`git-ssh`**.

### Install script (prebuilt release)

```bash
curl -fsSL https://raw.githubusercontent.com/kuyacarlo/ssh-profile/main/install.sh | sh
```

Override the install directory with `DEST` (default `/usr/local/bin`):

```bash
DEST="$HOME/.local/bin" curl -fsSL https://raw.githubusercontent.com/kuyacarlo/ssh-profile/main/install.sh | sh
```

### Fedora Copr

```bash
sudo dnf copr enable kuya-carlo/git-ssh
sudo dnf install git-ssh
```

### Prebuilt binaries

Download an archive from the [ssh-profile releases](https://github.com/kuyacarlo/ssh-profile/releases) page (`git-ssh_<os>_<arch>.tar.gz` or `.zip`), extract `git-ssh`, and place it on `$PATH`.

### Build from source

```bash
git clone https://github.com/kuyacarlo/ssh-profile.git
cd ssh-profile
make build
```

`make build` installs `git-ssh` to `$GOBIN` (or `$GOPATH/bin`). Confirm:

```bash
git-ssh version
# or: command -v git-ssh
```

---

## Paired quick start

Create matching profiles, then apply both in a repository:

```bash
# once per account
git-profile add alice          # interactive: name, email, signing key
git-ssh add alice              # creates ~/.ssh/git-ssh/alice/id_ed25519; print public key

# add the printed public key to GitHub → Settings → SSH keys

# existing checkout
cd /path/to/repo
git-profile use alice
git-ssh use alice

# or clone with the SSH profile applied
git-ssh clone alice private-repo
cd private-repo
git-profile use alice
```

Non-interactive git-profile example:

```bash
git-profile add work user.name "Jane Doe"
git-profile add work user.email jane.doe@company.example
git-profile add work user.signingkey ABCDEF0123456789
```

`git-ssh add` defaults:

- `github_user` = profile name (override with `--github-user`)
- `remote_host` = `github.com` unless set on the profile or in config
- identity = `~/.ssh/git-ssh/<profile>/id_ed25519` (created with `ssh-keygen -t ed25519` if missing)

Config locations:

| Tool | Default store |
| --- | --- |
| git-profile | `~/.gitprofile` |
| git-ssh | `~/.config/git-ssh/config.json` |
| git-ssh keys | `~/.ssh/git-ssh/<profile>/` |

---

## More usage

- git-profile commands, completion, and config flags: see the [git-profile README](../README.md).
- git-ssh commands (`clone`, `use`, `unuse`, `backup`, …): see the [ssh-profile README](https://github.com/kuyacarlo/ssh-profile#readme).
