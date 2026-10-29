# memlira-agent releases

Signed release files of the **Memlira agent**, the small program that runs next to a company's
own database and connects it to Memlira. This repository holds **no source code**: only GitHub
releases, `vX.Y.Z`, each with four files:

| File | What it is |
|---|---|
| `release.json` | The release manifest: version, the container image **by digest**, the tarball's URL and sha256 |
| `release.json.minisig` | Its minisign signature |
| `memlira-agent-vX.Y.Z-linux-x86_64.tar.gz` | The self-contained agent for Linux servers without Docker |
| `memlira-agent-vX.Y.Z-linux-x86_64.tar.gz.minisig` | Its minisign signature |

The container image is `ghcr.io/softriq/memlira-agent`, pulled by the digest in `release.json`.

## Verify before you install

Every file is signed with Memlira's release key, `memlira-agent-minisign.pub` in this repository.
A running agent checks the same key before it updates itself. This page is only a download place:
a file that was changed here fails the check.

```bash
v=X.Y.Z
base=https://github.com/softriq/memlira-agent-dist/releases/download/v$v
for f in release.json release.json.minisig \
         memlira-agent-v$v-linux-x86_64.tar.gz memlira-agent-v$v-linux-x86_64.tar.gz.minisig; do
  curl -fsSLO "$base/$f"
done
minisign -Vm release.json -p memlira-agent-minisign.pub
minisign -Vm memlira-agent-v$v-linux-x86_64.tar.gz -p memlira-agent-minisign.pub
grep -o '"sha256": *"[0-9a-f]*"' release.json      # = sha256sum of the tarball
sha256sum memlira-agent-v$v-linux-x86_64.tar.gz
```

The trusted comments read `memlira-agent vX.Y.Z` and `memlira-agent vX.Y.Z tarball`.

## Help

Install steps and support come with your Memlira account (the connection screen in the app).
Please don't open issues here: they are switched off.
