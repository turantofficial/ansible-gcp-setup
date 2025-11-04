# ansible-gcp-setup

Dynamically update Ansible inventory for GCP hosts using IAP plugin

Prerequisit:

`ansible-galaxy collection install google.cloud`

`gcloud auth application-default login`

Create **~/.ansible.cfg**: change your remote_user:

```
[defaults]
remote_user = your_gcp_user
private_key_file = ~/.ssh/google_compute_engine
inventory = ~/inventory.gcp.yml
interpreter_python = auto_silent
host_key_checking = False

[inventory]
enable_plugins = host_list, script, auto, yaml, ini, toml, google.cloud.gcp_compute
```

Create **~/inventory.gcp.yml**: change your remote_user!

```
plugin: google.cloud.gcp_compute
auth_kind: application
projects:
      - projA
      - projB
      - projC
filters:
  - status = RUNNING

hostnames:
  - name

keyed_groups:
  - key: project
  - prefix: project

compose:
  ansible_host: networkInterfaces[0].accessConfigs[0].natIP
  ansible_user: your_gcp_user
  ansible_ssh_private_key_file: ~/.ssh/google_compute_engine
```

`ansible-inventory -i ~/inventory.gcp.yml --graph`

e.g. ping monitoring VM

`ansible monitoring -i ~/inventory.gcp.yml -m ping`

e.g ping whole nlp_project:

`ansible nlp_project -i ~/inventory.gcp.yml -m ping`






