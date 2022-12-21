# QBlockChain English Installation Guide
![image](https://user-images.githubusercontent.com/101635385/208833565-1465d514-325e-4201-a622-1d83c90b75ea.png)

<h1 align="center"> QBlockChain-Testnet </h1>
<h1 align="center"> Hello,  QBlockChain-Testnet Incentivized Testnet Installation guide <br> by Hercules
</h1>

 ## 🟢 System Requirements

* 4 CPU <br> 8 GB RAM <br> 250 GB Disk Alanı

## 🟢 System Update
```shell
sudo apt update && sudo apt upgrade -y
```

## 🟢 Establishment of Necessary Libraries
```shell
apt install ca-certificates curl gnupg lsb-release git htop
```

## 🟢 Docker Setup
```shell

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io
docker version
```


## 🟢 Docker Compose Setup
```shell
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## 🟢 1. Download and Installation of Q Blockchain Files

```
screen -S qnetwork
git clone https://gitlab.com/q-dev/testnet-public-tools
```

## 🟢 Creating keystore Folder and pwd.txt File 

With the following command, we create a `keystore` folder in the `testnet-validator` file and a `pwd.txt` file in which we will write our password for our wallet to be given to us and enter this file. We write our password and save it with `ctrl x y enter` and exit.

```
cd testnet-public-tools/testnet-validator/
mkdir keystore
cd keystore
echo YOURPASS > keystore/pwd.txt
```

## 🟢 Wallet Creation
```
cd
cd testnet-public-tools/testnet-validator/
docker-compose run --rm --entrypoint "geth account new --datadir=/data --password=/data/keystore/pwd.txt" testnet-validator-node

```

After entering the above code, you should get an output like below. If you get such an output, everything is fine. 
```
Your new key was generated

Public address of the key:   0xb3FF24F818b0ff6Cc50de951bcB8f86b522aa  -  SİZE BÖYLE BİR MATEMASK ADRESİ VERECEK
Path of the secret key file: /data/keystore/UTC--2021-01-18T11-36-28.705754426Z--b3ff24f818b0ff6cc50de951bcb8f86b52287dac

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

## 🟢 installation configuration

```
cd
cd testnet-public-tools/testnet-validator/
cp .env.example .env
nano .env
```
We fill in the following places in the file.<br>
 - `METAMASK_ADRESS` in this section you write the wallet address given to you above without `0x` at the beginning.
 - `IP_ADRESI` you enter the ip address of your server.
 - Finally, save the file by pressing `ctrl x y enter`.

<br>

<img src="https://raw.githubusercontent.com/herculessx/Q-Network-Testnet/main/0aa05732-9d25-4a52-a4e1-aae61c6c659c.png" width="650">




## 🟢 ITN registration

 Register with the address given to you at the link below. The form will then give you an ITN number. Example: ITN-Hercules -12537 .  Add this number to your docker- compose.yaml file. 
 
 <br> 
 
 ![image](https://user-images.githubusercontent.com/101635385/208832860-0a7d9e3c-2424-4184-89df-07f7f7a514b9.png)

 
 * [Ödül Kayıt Linki](https://itn.qdev.li/)





## 🟢 Adding Our Validator to https://stats.qtestnet.org


```
nano docker-compose.yaml
```

We edit the following section in the file;
* `YOUR VALIDATOR_ID` in this section we write our validator name.

Copy the code below completely and replace it with the one in the docker-compose.yaml file where only the part that says YOUR VALIDATOR-NAME is changed and save. ctrl + x Yes and save

```
version: "3"

services:
  testnet-validator-node:
    image: $QCLIENT_IMAGE
    entrypoint: [
      "geth",
      "--testnet",
      "--datadir=/data",
      "--syncmode=full",
      "--ethstats=VALIDATOR_STATS_ID:qstats-testnet@stats.qtestnet.org",
      "--whitelist=3699041=0xabbe19ba455511260381aaa7aa606b2fec2de762b9591433bbb379894aba55c1",
      "--bootnodes=$BOOTNODE1_ADDR,$BOOTNODE2_ADDR,$BOOTNODE3_ADDR",
      "--verbosity=3",
      "--nat=extip:$IP",
      "--port=$EXT_PORT",
      "--unlock=$ADDRESS",
      "--password=/data/keystore/pwd.txt",
      "--mine",
      "--miner.threads=1",
      "--miner.gasprice=1",
      "--rpc.allow-unprotected-txs"
    ]
    volumes:
      - ./keystore:/data/keystore
      - ./additional:/data/additional
      - testnet-validator-node-data:/data
    ports:
      - $EXT_PORT:$EXT_PORT
      - $EXT_PORT:$EXT_PORT/udp
    restart: unless-stopped

volumes:
  testnet-validator-node-data:
```




## 🟢 Editing config.json File

We enter the file.
```
nano config.json
```
We organize the following places;
 - `METAMASK_ADRESI` in this section you write the wallet address given to you above without `0x` at the beginning.
 - `password` type your password
 - Finally, save the file by pressing `ctrl x y enter`.
```
 {
      "address": "METAMASK_ADRESS",
      "password": "YOURPASSWPRD",<br>
      "keystoreDirectory": "/data",
      "rpc": "https://rpc.qtestnet.org"
    }
```

<br>
<img src="https://github.com/herculessx/Q-Network-Testnet/blob/main/conf.png" width="650">
<br>



## 🟢 Staking Validatore

Remember to ask [faucetten](https://faucet.qtestnet.org/) for a token before doing this. 


```
docker run --rm -v $PWD:/data -v $PWD/config.json:/build/config.json qblockchain/js-interface:testnet validators.js
```




 
## 🟢 Run your node.
```
docker-compose up -d
```

## 🟢 Looking at Logs
```
docker-compose logs -f --tail "100"
```


##  synchronized

🟢 - synchronized, lots of peers
🟡 - synchronizing, there are several peers 
🔴 - not synchronized yet / few peers



## 🟢 Update

1- Stop Node and Delete Volume

```
docker-compose down -v
```
<br>
2- Download Latest Configurations

```
git pull
```
<br>
3- Pulling (and overwriting) the Latest Docker Container

```
docker-compose pull
```
<br>

4- Restart with New Configurations
```
docker-compose up -d
docker-compose logs -f --tail "100"
```

