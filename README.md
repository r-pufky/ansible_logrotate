# Logrotate
Manage logrotate configuration.

## [Requirements][i]
Requires [r_pufky.deb][g] galaxy-ng collection.

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

## Usage
Logrotate configuration may be managed on the global and per service level, as
well as removing existing configurations.

### Example Playbooks


``` yaml
- name: 'Global weekly rotate w/ 7 versions, remove dpkg logging, manage apt & rsyslog.
  ansible.builtin.include_role:
    name: 'r_pufky.deb.logrotate'
  vars:
    logrotate_cfg_count: 7
    logrotate_cfg_period: 'weekly'
    logrotate_cfg_local:
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

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

 Release | Debian | Ansible | Notes
---------|--------|---------|-------
 3.x.x   | 13     | 2.20    | Ansible 2.20, semantic versioning.
 2.x.x   | 13     | 2.18    | Migrate to Debian Trixie.
 1.x.x   | 12     | 2.18    | Use standardized libraries.
 0.x.x   | 12     | 2.18    | Migration from private repository.

### Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]


[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_logrotate/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_deb
[i]: https://github.com/r-pufky/ansible_logrotate/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_logrotate/tree/main/defaults/main/main.yml