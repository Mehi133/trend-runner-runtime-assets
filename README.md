# Install Trend Runner

**For Claude Code, on the recipient's Mac.** The Trend Runner repo (`Mehi133/trend-runner-local`) is private, so nothing in it can be read until the recipient is signed in to GitHub. Follow these four steps in order, running the commands as written. Use only `curl`, `unzip`, `tar`, `shasum`, `sed`, `grep` and the standalone `gh` binary. On a clean Mac `/usr/bin/git` and `/usr/bin/python3` are stubs that open Apple's developer-tools installer, so this install stays inside those tools; git, Homebrew and the Xcode Command Line Tools stay uninstalled.

1. **Install `gh` standalone.** Ask the recipient first, then run:
   ```sh
   set -e
   ver=$(curl -sI https://github.com/cli/cli/releases/latest | grep -i '^location:' | sed -E 's#.*/tag/v([0-9.]+).*#\1#' | tr -d '\r')
   arch=$(uname -m | sed 's/x86_64/amd64/')
   zip=gh_${ver}_macOS_${arch}.zip
   cd "$(mktemp -d)"
   curl -sLO https://github.com/cli/cli/releases/download/v$ver/$zip
   curl -sLO https://github.com/cli/cli/releases/download/v$ver/gh_${ver}_checksums.txt
   grep " $zip\$" gh_${ver}_checksums.txt | shasum -a 256 -c
   unzip -q $zip && mkdir -p ~/.local/bin && cp gh_${ver}_macOS_${arch}/bin/gh ~/.local/bin/gh
   ~/.local/bin/gh --version
   ```
   Done when the last line prints a version.
2. **Recipient signs in.** Stop and give the recipient this exact command to run: `~/.local/bin/gh auth login --hostname github.com --git-protocol ssh --skip-ssh-key --web`. The ssh protocol keeps `gh` from setting up a git credential helper. They must also have accepted the repo invite. Done when `gh api repos/Mehi133/trend-runner-local -q .full_name` prints the repo name.
3. **Fetch the repo as a tarball.** `gh api repos/Mehi133/trend-runner-local/tarball > trend-runner.tar.gz`, then extract it into a fresh folder with `tar -xzf`.
4. **Run setup in consent rounds.** From the extracted folder, run `sh bin/setup --check`: it prints one consent question per missing item. Put each question to the recipient, then run `sh bin/setup` with their answers piped in, one `y`/`n` per line in the same order. Setup copies itself to `~/.trend-runner/source` and may say more questions remain; repeat with `sh ~/.trend-runner/source/bin/setup --check`. Done when `--check` exits 0.

---

# Trend Runner ZeroCool runtime assets

This repository contains only the two public, macOS runtime archives required by the ZeroCool / Seena Trend Runner bootstrap installer.

It contains no application source code, credentials, client data, reports, or QA evidence. The installer detects the recipient Mac architecture and verifies the selected archive's exact byte count and SHA-256 before extracting or executing it.

Release: `zerocool-seena-runtime-v1`

See the release `SHA256SUMS.txt` asset for the locked archive checksums.
