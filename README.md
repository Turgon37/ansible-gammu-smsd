Ansible Role Gammu SMSD
========

This roles allow configuration of Gammu and Gammu SMSD utilities

## OS Family

This role is available for Debian

## Features

At this day the role can be used to configure :

  * Gammu cmdline utility
  * Gammu SMSD


## Configuration

The following parameters are available in the role. Some of them are well documented into gammu and gammu-smsd man pages :

| Name                              | Required ? | Description                                                                                                                    |
| ----------------------------------|----------- | ------------------------------------------------------------------------------------------------------------------------------ |
| gammu_smsd__gammu_connection      | mandatory  | The connection type between the host and the phone                                                                             |
| gammu_smsd__gammu_device          | mandatory  | The /dev/ device type to use to communicate with the phone                                                                     |
| gammu_smsd__gammu_model           | mandatory  | The model of the phone                                                                                                         |
| gammu_smsd__gammu_test_sms_number | optionnal  | If this parameter is set to a valid phone number, a SMS will be send to this number, after the successfull gammu configuration |

### Example of configuration

  * Nokia 3310 phone over fbus (serial UART type link with 3 cables)

```
gammu_smsd__gammu_connection: fbus
gammu_smsd__gammu_device: /dev/ttyAMA0
gammu_smsd__gammu_model: 3310
gammu_smsd__gammu_test_sms_number: "+33XXXXXXXXX"
```