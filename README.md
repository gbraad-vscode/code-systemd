Systemd service for code-cli
============================

Run code-cli serve web or tunnel using systemd. This has been part of my [`devenv`](https://github.com/gbraad-devenv/) installs, [`code-serveweb-action`](https://github.com/gbraad-actions/code-serveweb-action), and [`code-tunnel-action`](https://github.com/gbraad-actions/code-tunnel-action) actions.


### Install

#### [Install `code`](https://github.com/gbraad-dotfiles/upstream/blob/main/zsh/.zshrc.d/code.zsh)
```bash
target_arch="x64"   # "arm64"
target_path=/usr/bin/
tempfile=$(mktemp)
curl -fsSL "https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-${target_arch}" -o ${tempfile}
tar -zxvf ${tempfile} -C ${target_path} > /dev/null 2>&1
rm -f ${tempfile}
```

#### Services
```bash
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/code-serveweb%40.service \
  -o /etc/systemd/system/code-serveweb@.service
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/code-tunnel%40.service   \
  -o /etc/systemd/system/code-tunnelb@.service
sudo systemctl daemon-reload
sudo systemctl enable --now code-serveweb@${USER}
```
