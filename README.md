The vagrant file creates a three node k3s cluster and provision it with ansilbe

#### start cluster
By default the cluster uses the virtulbox private_network but it can also be setup to use the public_network

`vagrant up`

Once provisioning finishes run:
`ansible-playbook masters.yml -i inventory.yml`

This will install on the master node helm and prepare flux for gitops

#### requirements
`pip install sshpass`

