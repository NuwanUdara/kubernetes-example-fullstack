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
microk8s enable dns storage hostpath-storage metallb ingress
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
* nginxnp.yml
* tls.yml
* ing-svc.yml
* tls-ingress.yml


The NodePort will expose the application to the browser. There is a tls cirtificate in the tls.yml with base64 encoded. To create your own, use

```` basg
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=localhost"
````
This will create tls.crt and tls.key files. Get the data from the files with base64 encoded and without like wraps.

```` bash
cat tls.crt | base64 -w0
cat tls.key | base64 -w0
````
*note
The certificate is valid to used in localhost only.

After that, you can use your own tls data to be replaced in the tls.yml secret.

After that, check your connection with
``` bash
microk8s.kubectl get svc -n ingress
```
and grab the external ip address of the service 

```bash
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)         AGE
ingress   LoadBalancer   10.152.183.54   10.64.140.44   443:32559/TCP   8m6s
```

it will look somrthing like this. replace the 
https://10.64.140.44/ with your external ip address.

```bash
curl -k --header "Host: helloworld.local" https://10.64.140.44/
```

You will get a 200 response or, avaibale quotes stored in the mysql if there is any.

After that, to access the service from the browser,

```bash
sudo nano /etc/hosts
```

and add the host name and ip address to the file.

```bash
  GNU nano 6.2                       /etc/hosts                                 
127.0.0.1       localhost
127.0.1.1       nuwan-HP-ProBook-455-G7
10.64.140.44    helloworld.local

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

save it and, use https://helloworld.local to access the application. It may give warnings, just bypass them.