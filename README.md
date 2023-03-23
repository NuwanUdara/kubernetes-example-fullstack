# Fullstack Application Example with Kubernates

This project is a simple intergration of the docker compose example [in here](https://github.com/NuwanUdara/Docker-compose-example)
. 
The compose files and source data can be found here.

$How \;to \; deploy$

  You Have to install k8s in at least single node. I prefer microk8s. 

After that 
``` bash
microk8s kubectl create -f <file-name>
```
can be used to deploy content. To use Loadbalancer, storage and other service be sure to enable relevent addons.
After that 
``` bash
microk8s enable dns storage hostpath-storage metallb
```
The Order will not matter, the resources will wait till required resources are being created. 

But, the preferd order is,
* presistant_volume.yml
* pvc.yml
* pass-sec.yml
* initconfig.yml
* initnginx.yml
* mysql.yml
* back.yml
* front.yml
* nginx.yml
* nginxnp.yml (optional)

There is a Loadbalancer in the nginx.yml, but some services such as aws ec2, cannot use the metallb without high configureton. So, a nodeport is also provided.
