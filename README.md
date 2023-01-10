# QBlockChain English Installation Guide
![image](https://user-images.githubusercontent.com/101635385/208833565-1465d514-325e-4201-a622-1d83c90b75ea.png)

<h1 align="center"> QBlockChain-Testnet </h1>
<h1 align="center"> Hello,  QBlockChain-Testnet Incentivized Testnet Installation guide <br> by Hercules
</h1>

## 🟢 Link
### Explorer:
 * [Explorer](https://explorer.qtestnet.org/)
### Faucet:
 * [FAUCET](https://faucet.qtestnet.org/)
 
 ### Medium:
 * [Incentivized Guide Medium](https://medium.com/q-blockchain/q-blockchain-validator-onboarding-program-part-1-validator-incentivized-testnet-567ef6e4002e)

 * [Hercules Telegram](https://t.me/HerculesNode)
 * [Hercules Twitter](https://twitter.com/Hercules4413)
 * [QBlockChain Discord Channel](https://discord.gg/aYDmNjrsJC)
 * [QBlockChain Twitter Channel](https://twitter.com/QBlockchain)


 
 ## 🟢 System Requirements

* 4 CPU <br> 8 GB RAM <br> 250 GB Disk Alanı

## 🟢 System Update
```shell
sudo apt update
```

```shell
sudo apt upgrade
```

```shell
apt install docker-compose
```


## 🟢 Docker Setup
```shell

sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin

```

```shell
chmod +x /usr/local/bin/docker-compose

```

## 🟢 1. Download and Installation of Q Blockchain Files

```
git clone https://gitlab.com/q-dev/testnet-public-tools
```

## 🟢 Creating keystore Folder and pwd.txt File 

With the following command, we create a `keystore` folder in the `testnet-validator` file and a `pwd.txt` file in which we will write our password for our wallet to be given to us and enter this file. We write our password and save it with `ctrl x y enter` and exit.
<br><br>
`YOURPASSWORD` Type your password in the field.

```
cd testnet-public-tools/testnet-validator/
mkdir keystore
echo YOURPASSWORD > keystore/pwd.txt
```

## 🟢 Wallet Creation

Make sure you are in the `testnet-public-tools/testnet-validator/` directory.

<br>

```
docker-compose run --rm --entrypoint "geth account new --datadir=/data --password=/data/keystore/pwd.txt" testnet-validator-node

```

After entering the above code, you should get an output like below. If you get such an output, everything is fine. 
```
Your new key was generated

Public address of the key:   0xb3FF24F818b0ff6Cc50de951bcB8f86b522aa 
Path of the secret key file: /data/keystore/UTC--2021-01-18T11-36-28.705754426Z--b3ff24f818b0ff6cc50de951bcb8f86b52287dac

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

## 🟢 installation configuration  //  .env

Make sure you are in the `testnet-public-tools/testnet-validator/` directory.

<br>

```
cp .env.example .env
nano .env
```
We fill in the following places in the file.<br>
 - `ADDRESS` in this section you write the wallet address given to you above without `0x` at the beginning.
 - `IP` you enter the ip address of your server.
 - Finally, save the file by pressing `ctrl x y enter`.

<br>

![image](https://user-images.githubusercontent.com/101635385/208851568-d5db566d-bba1-4916-ac7e-bc66dcff921f.png)


## 🟢 Editing config.json File   //  .config.json

We enter the file.
```
nano config.json
```
We organize the following places;
 - `ADRESS` in this section you write the wallet address given to you above without `0x` at the beginning.
 - `password` type your password
 - Finally, save the file by pressing `ctrl x y enter`.
```
 {
      "address": "ADRESS",
      "password": "YOURPASSWPRD",<br>
      "keystoreDirectory": "/data",
      "rpc": "https://rpc.qtestnet.org"
    }
```

<br>

![image](https://user-images.githubusercontent.com/101635385/208851019-1f7b3a99-beb4-45b1-a98b-5c7d0085c024.png)

<br>



## 🟢 Metamask wallet import

Save the file starting with UTC in the keystore folder to your computer. Then open your Matemask wallet and import this file.
<br>
You can save your Utc file to your computer using Flezilla, winspc or Mobaxtrem.
<br>


![image](https://user-images.githubusercontent.com/101635385/209914837-0e997494-0e0b-469e-a5d5-2566ba3a9c2e.png)

![image](https://user-images.githubusercontent.com/101635385/209914718-a0531e08-fa14-47d3-ab31-d50f4bba839d.png)

<br>



## 🟢 ITN registration

 Register with the address given to you at the link below. The form will then give you an ITN number. Example: ITN-Hercules -12537 .  Add this number to your docker- compose.yaml file. 
 
 <br> 
 
 ![image](https://user-images.githubusercontent.com/101635385/208832860-0a7d9e3c-2424-4184-89df-07f7f7a514b9.png)

 
 * [Reward registration link](https://itn.qdev.li/)




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



## 🟢 Staking Validatore

Remember to ask [FAUCET](https://faucet.qtestnet.org/) for a token before doing this. 


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

🟢 - synchronized, lots of peers <br>
🟡 - synchronizing, there are several peers  <br>
🔴 - not synchronized yet / few peers <br>


<br>
<br>

* If you have completed the above steps, you do not need to do the Update part.


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


## 🟢 Error and solution Section ---- For the nodes stuck at block #3,699,041 

Open JS console with:
docker-compose exec testnet-validator-node geth attach /data/geth.ipc

Then set head to some blocks before:

```
debug.setHead(web3.toHex(3690000))
```

Close the console (ctrl+d) and restart the node:

```
docker-compose restart
```

You may have proper peers connected to your node (check the note below).
If you still face wrong block issue, try again one more time.
If still no success, try to resync from 0 block and check the note below how to get the correct peers:

```
docker-compose down -v --remove-orphans && docker-compose up -d

```


## 🟢 Steps to move your node to another server.

### 1-Stop your old server.

```shell
cd testnet-public-tools/testnet-validator/
docker-compose down -v
```

### 2- 

* With a program install Mobaxtrem or Flezilla or Winspc 

* My recommendation Mobaxtrem

* copy the `testnet-validator` folder to your computer. 

![image](https://user-images.githubusercontent.com/101635385/210863061-e7273a62-3466-42b7-a3d1-5f5f1df908a9.png)


### 3 - Install Testnet on your new server.


```shell
sudo apt update
```

```shell
sudo apt upgrade
```

```shell
apt install docker-compose
```

```shell

sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin

```


Download and Installation of Q Blockchain Files

```
git clone https://gitlab.com/q-dev/testnet-public-tools
```


```
cd testnet-public-tools
```

```
rm -fr testnet-validator
```

```
cd
```

### 4 - Install Testnet on your new server.

Upload the `testnet-validator` folder you backed up to your computer into the `testnet-public-tools` folder.

![image](https://user-images.githubusercontent.com/101635385/210862575-2edf5a90-ca37-44df-9b88-494c9e1a228a.png)


### 5 - Open your `.env` file and replace the ip address of your new server with the old one.

```
cd testnet-public-tools/testnet-validator/
```

save it after making the change.  Ctrl + X   Yes

```
nano .env 
```

![image](https://user-images.githubusercontent.com/101635385/210862182-39af482d-4a09-4231-ae69-e33ebe50badc.png)


### 6 - you can now run your node.

```
docker-compose up -d
docker-compose logs -f --tail "100"
```



## 🟢 Hercules
This is the installation <br>
Please don't forget to Fork and Like the guide. If you have any questions or concerns, you can find me on Q BlockChain Discord. 



