# Install Trend Runner

**For Claude Code, on the recipient's Mac.** The Trend Runner repo (`Mehi133/trend-runner-local`) is private, so nothing in it can be read until the recipient is signed in to GitHub. Follow these four steps in order. Use only `curl`, `unzip`, `shasum` and the standalone `gh` binary: git, Homebrew and the Xcode Command Line Tools stay uninstalled on this Mac (running `/usr/bin/git` opens Apple's installer).

1. **Install `gh` standalone.** Ask the recipient first. Download the latest `gh_<version>_macOS_<arm64|amd64>.zip` (match `uname -m`) from https://github.com/cli/cli/releases/latest, check its sha256 against that release's `gh_<version>_checksums.txt`, unzip it, and put `bin/gh` in `~/.local/bin`. Done when `~/.local/bin/gh --version` prints.
2. **Recipient signs in.** Stop and give the recipient this exact command to run: `~/.local/bin/gh auth login --hostname github.com --git-protocol ssh --skip-ssh-key --web`. The ssh protocol keeps `gh` from setting up a git credential helper. They must also have accepted the repo invite. Done when `gh api repos/Mehi133/trend-runner-local -q .full_name` prints the repo name.
3. **Fetch the repo as a tarball.** `gh api repos/Mehi133/trend-runner-local/tarball > trend-runner.tar.gz`, then extract it into a fresh folder with `tar -xzf`.
4. **Hand over to the repo.** From the extracted folder, run `sh bin/setup` and relay each consent question it asks to the recipient. It installs `uv` and Python, then the tool's own doctor asks about everything else. Done when `trend-runner doctor` reports every required item present.

---

# Trend Runner ZeroCool runtime assets

This repository contains only the two public, macOS runtime archives required by the ZeroCool / Seena Trend Runner bootstrap installer.

It contains no application source code, credentials, client data, reports, or QA evidence. The installer detects the recipient Mac architecture and verifies the selected archive's exact byte count and SHA-256 before extracting or executing it.

Release: `zerocool-seena-runtime-v1`

See the release `SHA256SUMS.txt` asset for the locked archive checksums.
