# Ansible Role: Counter-Strike: Source Death Match mode

An Ansible role that installs, upgrades and configures the deathmatch mode for Counter-Strike: Source.

## Requirements

An ansible role dedicated to the installation of SteamCMD such as [ansible-steamcmd](https://github.com/Aversiste/ansible-steamcmd).

An ansible role dedicated to the Installation of Metamod:Source such as [ansible-role-metamod-source](https://github.com/Aversiste/ansible-role-metamod-source).

An ansible role dedicated to the installation of a Source mod such as [ansible-role-cstrike-source](https://github.com/Aversiste/ansible-role-cstrike-source) or any role providing the `Restart {{ metamod_source_game }}` handler.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `steamcmd_user` | User name for steamcmd | `steam` |
| `cssdm_url` | Download mirror | `http://www.bailopan.net/cssdm/snapshots/2.1` |
| `cssdm_version` | Desired version | `2.1.6-git268` |
| `cssdm_target` | Archive name | `cssdm-{{ cssdm_version }}-linux.tar.gz` |
| `cssdm_install_path` | Installation directory | `/home/{{ steamcmd_user }}/.steam/steamapps/common/Counter-Strike Source Dedicated Server/cstrike` |
| `cssdm_cfg` | ... | ... |
| `cssdm_equip_cfg` | ... | ... |
| `cssdm_maps_cfg` | ... | ... |

# Dependencies

None.

# Example Playbook

```yaml
- hosts: game
  vars:
    cssdm_cfg: |
      cssdm_enabled "0"
    cssdm_maps_cfg:
      - map: de_dust_extended
        cfg: |
          cssdm_enabled "1"
      - map: de_dust2_unlimited
        cfg: |
          cssdm_enabled "1"
  pre_tasks:
    - package:
        name: acl
        state: present
  roles:
    - role: tleguern.steamcmd
    - role: tleguern.cstrike-source
    - role: tleguern.metamod-source
    - role: tleguern.sourcemod
    - role: tleguern.cssdm
```

# License

ISC

## Contributing

Either send [send GitHub pull requests](https://github.com/tleguern/ansible-role-cssdm) or [send patches on SourceHut](https://lists.sr.ht/~tleguern/misc).

# Author Information

Tristan Le Guern <tleguern@bouledef.eu>
