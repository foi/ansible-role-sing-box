# ansible-role-sing-box

highly customizable ansible role for [sing box](https://github.com/sagernet/sing-box).

<img src="https://github.com/foi/ansible-role-sing-box/actions/workflows/ci.yml/badge.svg?branch=main">

Compatibility
--------------

python: 3.12, 3.13, 3.14
ansible: 12, 13

Install
--------------

`ansible-galaxy role install foi.sing_box`

or add it in `requirements.yml`

```
roles:
  - name: foi.sing_box
```

and run `ansible-galaxy install -r requirements.yml`

Role Variables
--------------
```yml
sing_box_version: 1.13.11
sing_box_download_url: "https://github.com/SagerNet/sing-box/releases/download/v{{ sing_box_version }}/sing-box-{{ sing_box_version }}-linux-amd64-musl.tar.gz"
sing_box_tmp_path: /tmp
sing_box_binary_path: /usr/local/bin
sing_box_user: sing-box
sing_box_service_name: sing-box
sing_box_config_path: "/etc/{{ sing_box_service_name }}"
sing_box_config_mode: '0640'
sing_box_config: |
  define your own sing box config (here you may use jinja for templating)
# you can redifine this by using sing_box_unit_options: {}
sing_box_default_unit_options:
  Wants: network-online.target
  After: network-online.target
# you can redifine this by using sing_box_service_options: {}
sing_box_default_service_options:
  User: "{{ sing_box_user }}"
  Restart: always
  TimeoutStopSec: 5s
  LimitNOFILE: 1048576
  ProtectSystem: strict
  ProtectHome: tmpfs
  PrivateTmp: true
  PrivateDevices: true
  ProtectKernelTunables: true
  ProtectKernelModules: true
  ProtectKernelLogs: true
  ProtectControlGroups: true
  MemoryDenyWriteExecute: true
  LockPersonality: true
  RestrictRealtime: true
  ProtectClock: true
  RestartSec: 10
  StartLimitBurst: 30
  StartLimitIntervalSec: 30
  AmbientCapabilities: CAP_NET_ADMIN CAP_NET_RAW
  CapabilityBoundingSet: CAP_NET_ADMIN CAP_NET_RAW
```

Example Playbook
----------------
```yml
# inventory
[sing_box]
server
# playbook.yml
- hosts: sing_box
  roles:
    - role: foi.sing_box
  become: true
# host_vars/server.yml
sing_box_config: |
  {
    "log": {
      "level": "info",
      "output": "stdout"
    },
    "inbounds": [
      {
        "type": "socks",
        "listen": "0.0.0.0",
        "listen_port": 1080,
        "users": [
          {
            "username": "myuser",
            "password": "mypassword"
          }
        ]
      }
    ],
    "outbounds": [
      {
        "type": "direct",
        "tag": "direct"
      }
    ]
  }
```
License
-------

MIT
