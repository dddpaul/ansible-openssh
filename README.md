# OpenSSH server ansible role

Suitable to deploy https://github.com/dddpaul/docker-alpine-sshd

## Configuration

### User accounts

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

### Host SSH keys

Host SSH keys are specified with `openssh_keys` variable:

```yaml
openssh_keys:
  - filename: ssh_host_rsa_key
    content: "{{ lookup('file', 'keys/ssh_host_rsa_key') }}"
  - filename: ssh_host_rsa_key.pub
    content: "{{ lookup('file', 'keys/ssh_host_rsa_key.pub') }}"
    ...
```

These keys will be mounted inside container from `openssh_host_config_dir`. To mount existing keys from host `/etc/ssh` directory - just omit `content` field in `openssh_keys` items.

```yaml
openssh_keys:
  - filename: ssh_host_rsa_key
  - filename: ssh_host_rsa_key.pub
    ...
```

### Linux capabilities

Set `openssh_cap_add` and `openssh_cap_drop` to control container permissions. For example, grant permission to inspect processes (is necessary for using `lsof` inside container) with `CAP_SYS_PTRACE` capability.

```yaml
openssh_cap_add:
  - SYS_PTRACE
```

See https://man7.org/linux/man-pages/man7/capabilities.7.html for futher details.

### SSH configuration options

Set `openssh_options` dictionary with lowercased and underscored `/etc/ssh/sshd_config` options. For example:

```yaml
openssh_options:
  password_authentication: no # Becomes "PasswordAuthentication no" in /etc/ssh/sshd_config
```

But be awared that whole default dictionary value will be overriden.
So use `combine` filter to override few options but leave other with default values, for example:

```yaml
openssh_options: "{{ openssh_default_options | combine({
  'password_authentication': 'no',
  'client_alive_count_max': 5
}) }}"
```

See `man sshd_config` for detailed information about configuration options.
