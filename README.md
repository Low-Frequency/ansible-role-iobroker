# Ansible Role: ioBroker

Installs ioBroker on Debian/Ubuntu systems.

## Requirements

Node.js and npm have to be installed first. The easiest way to do this is to include [geerlingguy.nodejs](https://github.com/geerlingguy/ansible-role-nodejs) in the playbook.

## Role Variables

Avaliable variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):

```yaml
iob_dir: "/opt/iobroker"
```
The directory where ioBroker will be installed in. You shouldn't have to change this, but you can if you want. `iob_controller_dir` is relative to this directory and should not be changed.

```yaml
iob_user: "iobroker"
```
The user ioBroker gets executed as. Feel free to change this as you wish.
`iob_group` defaults to this value and shouldn't be changed.

```yaml
iob_groups:
  - audio
  - dialout
  - tty
  - video
```
The groups which the `iob_user` will be added to. These groups should suffice for most installations, but depending on your setup you might have to add some groups here. The default groups from the install script are as follows:
```
audio
bluetooth
dialout
gpio
i2c
redis
tty
video
```

```yaml
iob_service: "iobroker"
```
The name of the service ioBroker gets executed as. You might break something if you change this, but feel free to try it out.

```yaml
iob_cmd_links:
  - "/usr/bin/iob"
  - "/usr/bin/iobroker"
```
The global executables. Don't change them unless you know what you're doing.

```yaml
iob_npm_registry: "https://registry.npmjs.org"
```
The default `npm` registry used by the install script.

```yaml
iob_restore: false
```
Trigger variable for restoring a backup. You can use backups on the remote machine, or supply a backup via the `files` directory in the playbook folder.

```yaml
iob_use_local_backup: true
```
By default the restore will be done using a backup alredy located on the remote machine. Setting this to `false` will use a backup located in the `files` directory in the playbook folder.

```yaml
iob_backup_name: "iobroker-backup.tar.gz"
```
The filename of the backup supplied in the `files` folder. You have to change this if you set `iob_use_local_backup: false` or rename your backup file to `iobroker-backup.tar.gz`.

```yaml
iob_date: "{{ansible_date_time.year}}_{{ansible_date_time.month}}_{{ansible_date_time.day}}"
iob_time: "{{ansible_date_time.hour}}_{{ansible_date_time.minute}}_{{ansible_date_time.second}}"
iob_backup_file: "iobroker_{{iob_date}}-{{iob_time}}_backupiobroker.tar.gz"
```
These are only to create a file name that is registeres as a valid backup by ioBroker. Don't change them.

```yaml
iob_before_restore_commands:
```
A list of commands to execute before restoring the backup. You might need this if the restore doesn't work due to adapter version mismatches. If the playbook fails, it should give you the commands you need to set here. Example:
```yaml
iob_before_restore_commands:
  - "npm i iobroker.js-controller@4.0.15 --production"
```

```yaml
iob_use_custom_cert: false
```
Experimental. If you set this to `true`, the role will create `/opt/ssl` and upload a certificate and a private key supplied in the `files` directory in the playbook folder.

```yaml
iob_cert_folder: "/opt/ssl"
```
The folder where the certificate and private key will be copied to. You can change this if you want to.

```yaml
iob_cert_file: "iobroker.crt"
iob_private_key_file: "iobroker.key"
```
The names of the supplied certificate and private key files. Change them according to the files you want to use.

```yaml
iob_required_packages:
  - ...
```
I won't list the required packages here. It's only here to give a complete list of all variables used. You can add to this list if you want to install additional software, but don't remove anything from this list.

## Dependencies

Node.js and `npm` have to be installed before using this role. You can use [geerlingguy.nodejs](https://github.com/geerlingguy/ansible-role-nodejs) for this.

## Example Playbook

```yaml
---
- name: Install ioBroker
  hosts: iobroker

  roles:
    - geerlingguy.nodejs
    - iobroker
```

## License

MIT / BSD

## Author Information

This role was created in 2022 by [Low-Frequency](https://github.com/Low-Frequency).