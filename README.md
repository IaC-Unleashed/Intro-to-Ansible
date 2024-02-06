# Intro to Ansible #

![Imgur](https://i.imgur.com/73pb1Do.jpg)

## Overview ##

This project will be an introduction to configuration management with Ansible. We will first create a cloud server using Terraform, then we will configure that VM to be a webserver. This project assumes you have already followed the instructions detailed in [Intro to DigitalOcean and Terraform](https://github.com/IaC-Unleashed/Intro-to-DigitalOcean-and-Terraform), except do not destroy the virtual machine at the end. If you have, simply spin up another one!

---

## The *inventory* File ##

Before you can run the playbook, you must first set up the `inventory` file so that Ansible knows on which host to operate. The inventory file is protected by version control, so it is not checked into the repo. This INI file should be named `inventory`, exist in the `ansible` directory, and contain the following code:

```
[droplets]
iac-test-server ansible_host=<public_ip_server> ansible_connection=ssh ansible_user=root 
```

The value, `<public_ip_server>` is the IP address of the DigitalOcean droplet you created with Terraform in [Intro-to-DigitalOcean](https://github.com/IaC-Unleashed/Intro-to-DigitalOcean). Once the inventory is in place, you can run the following command to make sure it aligns with what is expected:

```shell
ansible-inventory -i inventory --list
```

The output should look like the following:

```shell
{
    "_meta": {
        "hostvars": {
            "iac-test-server": {
                "ansible_connection": "ssh",
                "ansible_host": "<public_ip_server>",
                "ansible_user": "root"
            }
        }
    },
    "all": {
        "children": [
            "ungrouped",
            "droplets"
        ]
    },
    "droplets": {
        "hosts": [
            "iac-test-server"
        ]
    }
}
```

You can also ping the hosts in the inventory by running:

```shell
$ ansible -i inventory -m ping all
```

An expected output should look like the following:

```shell
iac-test-server | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
---

