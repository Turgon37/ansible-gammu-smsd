Ansible Role Gammu/Gammu SMSD
=========

[![Build Status](https://travis-ci.org/Turgon37/ansible-gammu-smsd.svg?branch=master)](https://travis-ci.org/Turgon37/ansible-gammu-smsd)

:warning: This role is under development, some important (and possibly breaking) changes may happend. Don't use it in production level environments but you can eventually base your own role on this one :hammer:

:grey_exclamation: Before using this role, please know that all my Ansible roles are fully written and accustomed to my IT infrastructure. So, even if they are as generic as possible they will not necessarily fill your needs, I advice you to carrefully analyse what they do and evaluate their capability to be installed securely on your servers.

**This roles configure the gammu cli and/or the gammu-smsd daemon allow use of phone modem.**

## Features

Currently this role provide the following features :

  * gammu softwares installation
  * gammu and gammu-smsd configuration
  * [local facts](#facts)

## Requirements

### OS Family

This role is available for

  * Debian/Raspbian 8/9

### Dependencies

If you need to include the apt backports (to get newer version of usb_modeswitch) you will have to set need gammu_smsd__packages_allow_backports to True and to download the role [ansible-zabbix-agent](https://github.com/Turgon37/ansible-apt-backports)

## Role Variables

The variables that can be passed to this role and a brief description about them are as follows:

| Name                                 | Types/Values          | Description                                                                                        |
| -------------------------------------| ----------------------|----------------------------------------------------------------------------------------------------|
| gammu_smsd__facts                    | Boolean               | Install the local fact script                                                                      |
| gammu_smsd__service_configure_systemd| Boolean               | Install a custom systemd init script instead of init.d one to better understand daemon start errors|
| gammu_smsd__service_nice             | Integer from -20 to 19| Set custom nice on service                                                                         |

### Gammu specific variables

For each of theses variables you can have more informations by refering to man 5 gammurc

| Name                                 | Types/Values          | Description                                                                                        |
| -------------------------------------| ----------------------|----------------------------------------------------------------------------------------------------|
| gammu_smsd__gammu_connection         | String                | The type of connection to use with the modem (see man 5 gammu-smsdrc)                              |
| gammu_smsd__gammu_device             | String                | The path to the serial device that is linked to the modem (see man 5 gammu-smsdrc)                 |
| gammu_smsd__gammu_model              | String                | The optional model name (see man 5 gammu-smsdrc)                                                   |

### Gammu SMSD specific variables

For each of theses variables you can have more informations by refering to man 5 gammu-smsdrc

| Name                                 | Types/Values          | Description                                                                                       |
| -------------------------------------| ----------------------|-------------------------------------------------------------------------------------------------- |
| gammu_smsd__smsd_db_service          | String                | The type of backend to use as SMS storage                                                         |
| gammu_smsd__smsd_hook_receive        | String                | Path to an executable script able to dspatch SMS received event    (see man 5 gammu-smsdrc)       |
| gammu_smsd__smsd_hook_failure        | String                | Path to an executable script able to dspatch SMS init/sent failure event (see man 5 gammu-smsdrc) |
| gammu_smsd__smsd_hook_sent           | String                | Path to an executable script able to dspatch SMS sent event (see man 5 gammu-smsdrc)              |

:information_source: If you want to only configure gammu without running the gammu-smsd set gammu_smsd__service_enabled to False

### Testing purpose variables

| Name                                 | Types/Values           | Description                                                                             |
| -------------------------------------| -----------------------|-----------------------------------------------------------------------------------------|
| gammu_smsd__test_sms_number          | String                 | If set a valid phone number, ansible will try to send it a test message during playbook |

## Facts

By default the local fact are installed and expose the following variables :


* ```ansible_local.gammu.version_full```
* ```ansible_local.gammu.version_major```
* ```ansible_local.gammu_smsd.version_full```
* ```ansible_local.gammu_smsd.version_major```


## Example Playbook

To use this role create or update your playbook according the following example :


```
    - hosts: servers
      roles:
         - gammu-smsd
      vars:
        gammu_smsd__service_configure_systemd: True
        gammu_smsd__service_nice: -1
        gammu_smsd__gammu_connection: at19200
        gammu_smsd__gammu_device: /dev/ttyUSB0
        gammu_smsd__gammu_model: at
        gammu_smsd__smsd_smsc: '+33XXXXXXXX'
        gammu_smsd__smsd_hw_security: 0
        gammu_smsd__smsd_hook_receive: /opt/custom.sh 'receive'
        gammu_smsd__smsd_hook_failure: /opt/custom.sh 'failure'
        gammu_smsd__smsd_hook_sent: /opt/custom.sh 'sent'
```

### Phone configurations examples

  * Nokia 3310 phone connected over UART Serial Bus with the FBUS protocol (link established with a Raspberry Pi and a three wires link cable)

```
gammu_smsd__gammu_connection: fbus
gammu_smsd__gammu_device: /dev/ttyAMA0
gammu_smsd__gammu_model: 3310
```

  * Huawei E169 modem over USB

```
gammu_smsd__gammu_connection: at19200
gammu_smsd__gammu_device: /dev/ttyUSB0
gammu_smsd__gammu_model: at
```

## License

MIT