# Provision Grafana, InfluxDB and Python Scripts to 3 RaspberryPIs with Ansible

This repository is an fork of [ansible-pi](https://github.com/motdotla/ansible-pi), which is a nice starting point for Ansible automation.

**Whats included in this repository?**

* 3 playbooks to provision multiple RaspberryPIs
* apt update & upgrade & reboot your PIs
* start the Grafana service
* make a python script starting on boot and write an .ini file

**What will be provided in this repository soon?**

* install InfluxDB and Grafana using one command
* download a python script

## Usage

First, you must have installed [Ansible](http://www.ansible.com/).

Clone and setup the Ansible script. 

```
git clone https://github.com/ayeks/ansible-pi_grafana-influxdb-python.git
cd ansible-pi
cp hosts.example hosts
```

Edit the the `hosts` file to match your environment. Remove the vars if you are not using the [bme680_to_influxdb](https://github.com/ayeks/bme680_to_influxdb) script for sending temperature, humidity and gas readings to InfluxDB.


**Update & Upgrade & Reboot**

If you just want to update & upgrade your RaspberryPIs with Ansible execute:

```
ansible-playbook playbook-update.yml -i hosts --ask-pass --become -c paramiko
```


**Enable autostart on boot & write .ini file**

If you would like to start a python script on boot time and write an .ini file with variables execute:

```
ansible-playbook playbook-setup-node.yml -i hosts --ask-pass --become -c paramiko

```

**Run everything**

If you just want to execute all playbooks run:
```
ansible-playbook playbook.yml -i hosts --ask-pass --become -c paramiko
```

Which results in the following output for my setup:

```
$ ansible-playbook playbook.yml -i hosts --ask-pass --become -c paramiko
SSH password: 

PLAY [Execute update, upgrade and reboot] **********************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [192.168.178.54]
ok: [192.168.178.73]
ok: [192.168.178.66]

TASK [common : set_fact] ***************************************************************************************************************************************************
ok: [192.168.178.54]
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [common : Update APT package cache] ***********************************************************************************************************************************
changed: [192.168.178.73]
changed: [192.168.178.66]
changed: [192.168.178.54]

TASK [common : Upgrade APT to the lastest packages] ************************************************************************************************************************
changed: [192.168.178.54]
changed: [192.168.178.66]
changed: [192.168.178.73]

TASK [common : Reboot] *****************************************************************************************************************************************************
changed: [192.168.178.54]
changed: [192.168.178.66]
changed: [192.168.178.73]

TASK [common : Wait for Raspberry PI to come back] *************************************************************************************************************************
ok: [192.168.178.54 -> localhost]
ok: [192.168.178.66 -> localhost]
ok: [192.168.178.73 -> localhost]

PLAY [Ansible Playbook for configuring the webserver with InfluxDB and Grafana] ********************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [192.168.178.54]

TASK [grafana : set_fact] **************************************************************************************************************************************************
ok: [192.168.178.54]

TASK [grafana : service] ***************************************************************************************************************************************************
ok: [192.168.178.54]

PLAY [Write config.ini and reboot playbook] ********************************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************************************************************
ok: [192.168.178.73]
ok: [192.168.178.66]

TASK [nodes : set_fact] ****************************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : remove senddata.py exec to rc.local] *************************************************************************************************************************
changed: [192.168.178.66]
changed: [192.168.178.73]

TASK [nodes : write senddata.py exec to rc.local] **************************************************************************************************************************
changed: [192.168.178.66]
changed: [192.168.178.73]

TASK [nodes : Set influxserver host] ***************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set influxserver port] ***************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set influxserver user] ***************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set influxserver password - not a good idea.. FIXME TODO] ****************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set influxserver dbname] *************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor session] ******************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor location] *****************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor enable_gas] ***************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor temp_offset] **************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor interval] *****************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Set sensor burn_in_time] *************************************************************************************************************************************
ok: [192.168.178.66]
ok: [192.168.178.73]

TASK [nodes : Reboot] ******************************************************************************************************************************************************
changed: [192.168.178.66]
changed: [192.168.178.73]

TASK [nodes : Wait for Raspberry PI to come back] **************************************************************************************************************************
ok: [192.168.178.66 -> localhost]
ok: [192.168.178.73 -> localhost]

PLAY RECAP *****************************************************************************************************************************************************************
192.168.178.54             : ok=9    changed=3    unreachable=0    failed=0   
192.168.178.66             : ok=23   changed=6    unreachable=0    failed=0   
192.168.178.73             : ok=23   changed=6    unreachable=0    failed=0   

```
