# OpenSSH server ansible role

Suitable to deploy https://github.com/dddpaul/docker-alpine-sshd

## Configuration

Users accounts are specified with `openssh_users` variable:

```yaml
openssh_users:
  login1:
    password_hash: xxx
    ssh_public_key: yyy
  login2:
    ...
```

`openssh_users` is not defined by default, in this case it composed from yaml-files in `openssh_users_dir` directory.
