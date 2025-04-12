Systemd services for `code` / `codium`
======================================

Run `code tunnel`, `code serve-web`  or `codium serve-web` using systemd. This has been part of my [`devenv`](https://github.com/gbraad-devenv/) installs, [`code-serveweb-action`](https://github.com/gbraad-actions/code-serveweb-action), and [`code-tunnel-action`](https://github.com/gbraad-actions/code-tunnel-action) actions.


### Install Code

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
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-serveweb%40.service \
  -o /etc/systemd/system/code-serveweb@.service
sudo curl -fsSL https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/system/code-tunnel%40.service   \
  -o /etc/systemd/system/code-tunnel@.service
sudo systemctl daemon-reload
sudo systemctl enable --now code-serveweb@${USER}
```


#### Services (user)
```bash
mkdir -p ~/.config/systemd/user/
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-serveweb.service \
  -o ~/.config/systemd/user/code-serveweb.service
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/user/code-tunnel.service   \
  -o ~/.config/systemd/user/code-tunnel.service
systemctl --user daemon-reload
systemctl --user enable --now code-serveweb
```

> [!NOTE]
> You might need to run `loginctl enable-linger ${USER}` to allow processes to remain running after you log out.


### Install Codium Server

Download the latest version as described in [this commit](https://github.com/gbraad-dotfiles/upstream/commit/9f186490077e03f89e5853ce61fea1097fc21f87#diff-2f571aaa8c76bed87f54f6ff849b8b7dca279829a5ab56bfdca488687382b5c9) and related [issue](https://github.com/gbraad-devenv/fedora/issues/77).


#### Services (system)
```bash
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-system/codium-server%40.service \
  -o /etc/systemd/system/codium-server@.service
sudo systemctl daemon-reload
sudo systemctl enable --now codium-server@${USER}
```


#### Services (user)
```bash
mkdir -p ~/.config/systemd/user/
curl -fsSL  https://raw.githubusercontent.com/gbraad-vscode/code-systemd/refs/heads/main/codium-user/codium-server.service \
  -o ~/.config/systemd/user/codium-server.service
systemctl --user daemon-reload
systemctl --user enable --now codium-server
```
