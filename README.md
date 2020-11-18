# Ansible playbook to manage TP-LINK switches over telnet <!-- omit in toc -->
- [Background](#background)
  - [Supported switch models](#supported-switch-models)
  - [Scope](#scope)
  - [Invocation](#invocation)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
  - [Configuration](#configuration)
  - [Running](#running)
    - [Show mirroring status](#show-mirroring-status)
    - [Enable mirroring](#enable-mirroring)
    - [Enable mirroring on custom ports](#enable-mirroring-on-custom-ports)
    - [Mirror several ports](#mirror-several-ports)
    - [Stop mirroring](#stop-mirroring)
- [Known issues](#known-issues)

## Background 

Business-grade TP-Link switches support various methods of administration: via Web interface, SNMP, and over telnet.

This playbook manages switch over telnet protocol.

The playbook also includes modified version of stock `telnet` plugin, as all telnet commands are sent with `\n` terminator character while TP-Link expects `\r` terminator.

### Supported switch models

Tested on:
* `T1500G-10MPS`


### Scope

Currently, only port mirroring is supported. 
Extending for additional functionality is trivial: see the `port_mirror` role.

### Invocation

Should be invoked from a host where you would run a telnet session. TP-Link IP address should be reachable from this box.


## Prerequisites

Install `ansible` if not yet installed, following [instructions for your OS](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

## Usage

### Configuration

Set your switch IP in the `hosts` file.

### Running

#### Show mirroring status
```bash
ansible-playbook -i hosts playbooks/portmirror.yml -v --tags=show
```
#### Enable mirroring
```bash
ansible-playbook -i hosts playbooks/portmirror.yml -v --tags=mirror
```
#### Enable mirroring on custom ports
```bash
ansible-playbook -i hosts playbooks/portmirror.yml -v --tags=mirror --extra-vars="from_port=1/0/1 to_port=1/0/2"
```
#### Mirror several ports
```bash
ansible-playbook -i hosts playbooks/portmirror.yml -v --tags=mirror --extra-vars="from_port=1/0/1 to_port=1/0/2"

ansible-playbook -i hosts playbooks/portmirror.yml -v --tags=mirror --extra-vars="from_port=1/0/3 to_port=1/0/2"
```
#### Stop mirroring
```bash
ansible-playbook -i hosts playbooks/portmirror.yml -v  --tags=unmirror
```

## Known issues

Sometimes switch prints out two log lines mixed together (from concurrent threads). In that case, `playbook` fails to find expected line and bails out with an error. 
 
 In this, just re-run the task.






