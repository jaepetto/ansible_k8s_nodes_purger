# Kubernetes nodes purger

## Description

This is a simple Ansible playbook to purge Kubernetes nodes from a cluster.

It aims at:

- cordon and drain the node
- list and delete all pods on the node
- list and delete all images on the node
- reboot the server
- uncordon the node

## Requirements

It requires Ansible (at least 7.3), Kubectl and SSH access to the nodes.

## installation

The easiest path is to use a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

## Environment variables

5 environment variables are controlling the behavior of the playbook:
| name                          | description                                   | default value |
| ----------------------------- | --------------------------------------------- | ------------- |
| `drain_nodes`                 | Whether to drain the nodes or not             | 0             |
| `kill_containers`             | Whether to kill the running containers or not | 0             |
| `remove_images`               | Whether to remove the images or not           | 1             |
| `reboot_server`               | Whether to reboot the server or not           | 0             |
| `max_number_of_parallel_jobs` | The maximum number of parallel jobs           | 1             |

## Usage

You should first provide the list of nodes ot purge in the `inventory/prod` file:

```bash
kubectl get nodes | grep -v 'control-plane,master' | awk '{print $1}' | tail -n +2 > inventory/prod
```

Then you can run the playbook:

```bash
ansible-playbook -i inventory/prod playbooks/main.yml
```
