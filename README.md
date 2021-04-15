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
| `cssdm_cfg` | Main configuration file content | See bellow |
| `cssdm_equip_cfg` | Equipment configuration file content | See bellow |
| `cssdm_maps_cfg` | Map specific configuration files content | See bellow |

### `cssdm_cfg`

This variable holds the content of the main configuration file for the deathmatch server plugin located at `cfg/cssdm/cssdm.cfg`.

Example:

```yaml
cssdm_cfg: |
  cssdm_enabled "1"
  cssdm_ffa_enabled "0"
  cssdm_spawn_method "preset"
  ...
```

Default values are available [here](https://github.com/alliedmodders/cssdm/blob/master/cfg/cssdm.cfg).

### `cssdm_equip_cfg`

This variable holds the content of the secondary configuration file for the deathmatch server plugin located at `cfg/cssdm/cssdm.equip.txt`.

Example:

```yaml
cssdm_equip_cfg: |
  "Equipment"
  {
    "Settings"
    {
      "guns_command" "yes"
    }
    "AutoItems"
    {
      "health" "100"
      "armor" "100"
      "helmet" "yes"
      "flashbangs" "0"
      "smokegrenade" "no"
      "hegrenade" "no"
      "defusekits" "yes"
      "nightvision" "yes"
    }
    ...
  }
```

Default values are available [here](https://github.com/alliedmodders/cssdm/blob/master/cfg/cssdm.equip.txt).

### `cssdm_maps_cfg`

This variable allows to apply specific configurations to some maps.
It is particularly useful for servers allowing other game modes in conjunction to deathmatch.

The structure is a list of dictionaries.

| Key | Description |
| --- | ----- |
| `map` | Name of the map |
| `cfg` | Specific configuration for this map |
| `equip` | Specific equipment configuration for this map |

Example:

```yaml
cssdm_maps_cfg:
  - map: fy_garage
    cfg: |
      cssdm_enabled "1"
      cssdm_ffa_enabled "1"
      cssdm_respawn_command "1"
  - map: de_dust2_unlimited
    cfg: |
      cssdm_enabled "1"
      cssdm_ffa_enabled "0"
      cssdm_respawn_command "0"
    equip: |
      "Equipment"
      {
        "Settings"
        {
          "guns_command" "no"
        }
        "Menus"
        {
          "primary" "no"
          "secondary" "no"
          "buy" "yes"
        }
        "AutoItems"
        {
          "health" "100"
          "armor" "0"
          "helmet" "no"
          "flashbangs" "0"
          "smokegrenade" "no"
          "hegrenade" "no"
          "defusekits" "no"
          "nightvision" "no"
        }
      }
```

## Dependencies

None.

## Example Playbook

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

## License

ISC

## Contributing

Either send [send GitHub pull requests](https://github.com/tleguern/ansible-role-cssdm) or [send patches on SourceHut](https://lists.sr.ht/~tleguern/misc).

## Author Information

Tristan Le Guern <tleguern@bouledef.eu>
