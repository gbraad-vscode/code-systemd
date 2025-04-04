Systemd services for `code`
===========================

Run code-cli serve web or tunnel using systemd. This has been part of my [`devenv`](https://github.com/gbraad-devenv/) installs, [`code-serveweb-action`](https://github.com/gbraad-actions/code-serveweb-action), and [`code-tunnel-action`](https://github.com/gbraad-actions/code-tunnel-action) actions.


### Install

#### [Install `code`](https://github.com/gbraad-dotfiles/upstream/blob/main/zsh/.zshrc.d/code.zsh)
```bash
target_arch="x64"      # "arm64"
target_path=/usr/bin/  # ~/.local/bin/
tempfile=$(mktemp)
curl -fsSL "https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-${target_arch}" -o ${tempfile}
tar -zxvf ${tempfile} -C ${target_path} > /dev/null 2>&1
rm -f ${tempfile}
```


#### Or package (RPM)
```bash
target_arch="x64"      # "arm64"
tempfile=$(mktemp)
curl -fsSL "https://code.visualstudio.com/sha/download?build=stable&os=linux-rpm-${target_arch}" -o ${tempfile}
rpm -ivh ${tempfile}
rm -f ${tempfile}
```


#### Services (system)
```bash
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/system/code-serveweb%40.service \
  -o /etc/systemd/system/code-serveweb@.service
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/system/code-tunnel%40.service   \
  -o /etc/systemd/system/code-tunnel@.service
sudo systemctl daemon-reload
sudo systemctl enable --now code-serveweb@${USER}
```


#### Services (user)
```bash
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/user/code-serveweb.service \
  -o ~/.config/systemd/user/code-serveweb.service
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/codecli-systemd/refs/heads/main/user/code-tunnel.service   \
  -o ~/.config/systemd/user/code-tunnel.service
systemctl --user daemon-reload
systemctl --user enable --now code-serveweb
```
