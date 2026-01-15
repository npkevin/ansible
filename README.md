# Ansible for my Homelab 📖

This repo manages my homelab infrastructure with Ansible: base OS hardening,
user provisioning, Docker hosts, DNS, SMB storage, and admin tooling. It is
organized around host groups and targeted playbooks.

## Inventory at a glance

| Group | Hosts | Purpose |
| --- | --- | --- |
| nas | `stoxnas.kevnp.lan` | NAS / storage services |
| dns | `netxdns.kevnp.lan` | DNS resolver (Unbound) |
| adm | `labxadm1.kevnp.lan`, `labxadm2.kevnp.lan` | Admin workhorses |
| media | `medxarr.kevnp.lan` | Media stack |
| minecraft | `minecraft.kevnp.lan` | Game server |

## Playbooks

| Playbook | Targets | Roles |
| --- | --- | --- |
| `servers/adm.yml` | `adm` | `ansible`, `terraform`, `docker` |
| `servers/dns.yml` | `dns` | `unbound` |
| `servers/stoxnas/setup.yml` | `nas` | `samba`, `stoxnas` |
| `servers/medxarr/setup.yml` | `media` | `docker`, `medxarr` |
| `servers/minecraft/setup.yml` | `minecraft` | `docker`, `minecraft` |
| `servers/provision.yml` | `all` | `base` (Terraform bootstrap) |

## Roles

| Role | Purpose |
| --- | --- |
| `base` | Base provisioning, users, packages |
| `ansible` | Ansible host tooling |
| `terraform` | Terraform tooling |
| `docker` | Docker host setup |
| `samba` | SMB services |
| `unbound` | DNS resolver |

## Post-Provision Setups

```bash
# Admin hosts
ansible-playbook servers/adm.yml

# Unbound DNS hosts
ansible-playbook servers/dns.yml

# Media (limit if needed)
ansible-playbook servers/medxarr/setup.yml -l medxarr.kevnp.lan
```

## Provisioning flow

`servers/provision.yml` is intended for Terraform-driven bootstrapping. It uses
root access for first run, sets base users/packages, and accepts dynamic disks
via `terraform_disks`.

## Repo layout

```
ansible.cfg
inventory
group_vars/
host_vars/
roles/
servers/
```
