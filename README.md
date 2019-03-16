# Kube Cluster on Cloud!

This a project to automate the installation of a kubernetes cluster on Hetzner Cloud (VMs) (although it should be provider agnostic, as it relies on an inventory of already deployed VMs)

Reference: https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-1-10-cluster-using-kubeadm-on-ubuntu-16-04

## Infrastructure

| VM name     | Type | Disk  | Class    | IP              |
| ----------- | ---- | ----- | -------- | --------------- |
| kube-node2  | CX11 | 20 GB | nbg1-dc3 |                 |
| kube-node1  | CX11 | 20 GB | nbg1-dc3 |                 | 
| kube-master | CX11 | 20 GB | nbg1-dc3 |                 | 

## Steps

1. Edit the `hosts` inventory file and set the IPs

```
cp hosts.dist hosts
vim hosts
```

2. Run the initial step

```
ansible-playbook -i hosts ./initial.yaml
```

3. Edit `kube-dependencies.yaml` file to change the version of the `kube tools` to the desired kubernetes version.

4. Run `kube-dependencies.yaml`

```
ansible-playbook -i hosts ./kube-dependencies.yaml
```

5. Run `master.yaml`

```
ansible-playbook -i hosts ./master.yaml
```

6. Run `workers.yaml`

```
ansible-playbook -i hosts ./workers.yaml
```

7. Wait a bit, will take up to 5 minutes for the cluster to setup

8. Test with a simple app (check the reference link for a nginx example)

## Dashboard!

From the master... (or wherever you have a configured `kubectl`)

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

TODO: add to ansible?

P.D.: If using the `master` node, remember to run that command with the `kuberenetes` user...

## Ansible and KnownHosts check of SSH

You could set this env variable on the call to `initial.yaml`...

```
ANSIBLE_HOST_KEY_CHECKING=false
```
