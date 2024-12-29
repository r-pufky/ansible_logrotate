# Logrotate
Manage logrotate configuration.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_logrotate/blob/main/meta/main.yml)

## Role Variables
[defaults](https://github.com/r-pufky/ansible_logrotate/blob/main/defaults/main.yml)

## Dependencies
Part of the [r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv)
collection.

## Example Playbook
Logrotate configuration may be managed on the global and per service level, as
well as removing existing configurations.

This will set weekly rotations with seven versions globally, remove logrotate
for `dpkg` and manage the configuration for `apt` and `rsyslog`.

host_vars/client.example.com/vars/logrotate.yml
``` yaml
logrotate_count: 7
logrotate_period: 'weekly'
logrotate_local:
  - service: 'dpkg'
    state: 'absent'
  - service: 'apt'
    state: 'present'
    config:
      - logs:
          - '/var/log/apt/term.log'
        rotate:
          count: 12
          period: 'monthly'
          compress: true
          missing_ok: true
          if_empty: false
      - logs:
          - '/var/log/apt/history.log'
        rotate:
          count: 12
          period: 'monthly'
          compress: true
          missing_ok: true
          if_empty: false
  - service: 'rsyslog'
    state: 'present'
    configs:
       - logs:
           - '/var/log/syslog'
           - '/var/log/mail.info'
           - '/var/log/mail.warn'
           - '/var/log/mail.err'
           - '/var/log/mail.log'
           - '/var/log/daemon.log'
           - '/var/log/kern.log'
           - '/var/log/auth.log'
           - '/var/log/user.log'
           - '/var/log/lpr.log'
           - '/var/log/cron.log'
           - '/var/log/debug'
           - '/var/log/messages'
         rotate:
           count: 4
           period: 'weekly'
           missing_ok: true
           if_empty: false
           compress: true
           delay_compress: true
           shared_scripts: true
           post_rotate: '/usr/lib/rsyslog/rsyslog-rotate'
```

Apply the role
``` yaml
- name: 'Manage logrotate'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.logrotate'
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_srv/blob/main/docs/dev/environment/README.md)

Run all unit tests:
``` bash
molecule test --all
```

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_logrotate/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
