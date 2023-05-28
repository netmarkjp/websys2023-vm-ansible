# WEBSYS2023 VMセットアップ

ガイダンスでのVMセットアップを模したAnsible Playbook

## 実行の前提条件

- 接続ユーザがsudoできる
- 自分の環境に合わせて `ssh_config` を調整する

## Ansible実行環境構築例

```sh
sudo apt -y update
sudo apt -y install python3-venv
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip wheel
pip install -r requirements.txt
```

## 実行方法

```sh
# linting
ansible-lint site.yml -p -x 401

# check connectivity
ansible -m ping -i hosts --ssh-common-args="-F ssh_config" all

# dry run
ansible-playbook -i hosts --ssh-common-args="-F ssh_config" site.yml -C

# apply
ansible-playbook -i hosts --ssh-common-args="-F ssh_config" site.yml
```

※パスワード認証の場合は `sshpass` をインストールしたうえで `--ask-pass` `--ask-become-pass` つきで実行する

```sh
sudo apt -y install sshpass
ansible --ask-pass -m ping -i hosts --ssh-common-args="-F ssh_config" all
ansible-playbook --ask-pass --ask-become-pass -i hosts --ssh-common-args="-F ssh_config" site.yml -C
ansible-playbook --ask-pass --ask-become-pass -i hosts --ssh-common-args="-F ssh_config" site.yml
```

