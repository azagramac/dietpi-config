<p align="center">
  <img src="https://github.com/user-attachments/assets/43726c51-3279-4c5f-98a7-2cbbc1f8bf46" width="200" title="nanopi_logo">
  <img src="https://github.com/user-attachments/assets/f341ce45-3a22-4203-9bb7-fe389014d980" width="200" title="dietpi_logo">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/207cc972-96da-411b-8487-e511b7dad1be" width="150" title="swarm_logo">
</p>


---
DietPi, bookworm: [v9.9](https://dietpi.com/downloads/images/DietPi_NanoPiNEO3-ARMv8-Bookworm.img.xz)\
sha256: `046599f364ec42edaa8477652ea6c934000ce5e7cba656b37dc6845810dba21c`

---
Cluster docker swarm\
2x NanoPi Neo3 (model 2Gb ram) (one more coming soon)\
2x 64Gb MicroSD\
1x Switch TP-Link TL-SG1008P

![image](https://github.com/user-attachments/assets/f4e75ed6-3671-417c-b15b-ca7cb51d61e9)

![image](https://github.com/user-attachments/assets/2786862c-0014-4178-92d9-12a8f8084197)

---
#### packages master
```bash
apt update && apt install git vim wget curl dbus net-tools ca-certificates gnupg -y
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
apt install git vim curl wget dbus net-tools nfs-common ca-certificates gnupg build-essential cmake libssl-dev neofetch -y
apt install cups libcups2-dev avahi-daemon -y
apt install ssl-cert xfonts-utils xfonts-encodings fonts-dejavu-core fontconfig-config libnss3 -y
```

#### packages worker
```bash
apt update && sudo apt install git vim curl wget dbus net-tools nfs-common ca-certificates gnupg build-essential libssl-dev neofetch -y
apt update && sudo apt install git vim wget curl net-tools ca-certificates gnupg -y
apt update && sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

#### Gen SSH key, and copy key booth nodes. 
```bash
ssh-keygen -t rsa -b 4096
...
ssh-copy-id -i .ssh/id_rsa.pub root@{ip_node}
```

#### /etc/fstab
```bash
{ip_nfs_server}:{path_shared}	{path_mount}	nfs4	intr,timeo=100,_netdev,rw	0	0
```

#### Swarm init, only master, for join workers nodes
```bash
docker swarm init --advertise-addr {ip_master}
```
<sup><em>This command is executed only once on the master node.</sup></em>

To add a worker to this swarm, run the following command:
```bash
docker swarm join --token {token} {ip_master}:2377
```
<sup><em>Execute the command on each node you want to use as a worker.</sup></em>

#### Join managers
```bash
docker swarm join-token manager
```
<sup><em>This command is executed only once on the master node.</sup></em>

To add a manager to this swarm, only managers run the following command:
```bash
docker swarm join --token {token} {ip_master}:2377
```
<sup><em>Execute the command on each node you want to use as a manager.</sup></em>

#### Deployment services. 
```bash
docker stack deploy -c docker-compose.yaml {name}
```
<sup><em>This command is executed only once on the master node.</sup></em>

### Settings dietpi-config
```bash
Display Options > LED Control > mmc0
```

---
<img src="https://github.com/user-attachments/assets/bcd104e6-a2db-4f62-b507-f894b1c1d08f" width="700" title="terminal_master">

<img src="https://github.com/user-attachments/assets/282ed869-33ce-4c79-a5d6-6adf077b1a65" width="700" title="terminal_worker">

---
Hardware Spec

<img src="https://github.com/user-attachments/assets/006f536d-67d7-4f74-a7f6-eb119a2aafc4" width="740" title="nanopi_neo3_board">
