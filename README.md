![666b297b8d80555ff9a25256_allora-points-phase-2](https://github.com/0xmoei/allora-testnet/assets/90371338/6298f73a-3c58-40a6-9d92-725f36456901)

<h1 align="center">Allora Network Point Program</h1>

> - Create a new wallet in Keplr
>
> - Connect to the on-chain Point Program [Dashboard](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6IjVlNmEwMjc5LTcxNjEtNDhmYS04NGM3LWEzYzM0MGM4MGIzNyJ9)
>
> - In Campaigns tab you see 2 tasks, Check them
> 
> - In the tutorial we run 3 `Price Prediction Workers` with `topic 1,2,7` (Predicting `ETH` price every 24hr & 1hr & 20 mins)
>
> - Check the campaigns tasks steps to see what `topic` means
>
> - We get points by running a worker

#

> Make sure to join off-chain community tasks on [Zealy](https://zealy.io/cw/alloranetwork/invite/IU2cqqMstYG1pEtHTenpn) & [Galxe](https://app.galxe.com/quest/AlloraNetwork) since they are as important as onchain tasks
>
> Team will add new tasks in it this week

#

<h1 align="center">Price Prediction Worker Node</h1>

## System Requirements
![image](https://github.com/0xmoei/allora-testnet/assets/90371338/56f1e0d2-4d59-436c-a0e0-183f9a082de4)

## Install dependecies
```console
# Install Packages
sudo apt update & sudo apt upgrade -y

sudo apt install ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev curl git wget make jq build-essential pkg-config lsb-release libssl-dev libreadline-dev libffi-dev gcc screen unzip lz4 -y
```
```console
# Install Python3
sudo apt install python3
python3 --version

sudo apt install python3-pip
pip3 --version
```
```console
# Install Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
docker version

# Install Docker-Compose
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)

curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version

# Docker Permission to user
sudo groupadd docker
sudo usermod -aG docker $USER
```
```console
# Install Go
sudo rm -rf /usr/local/go
curl -L https://go.dev/dl/go1.22.4.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> $HOME/.bash_profile
source .bash_profile
go version
```

## Install Allorad: Wallet
```console
git clone https://github.com/allora-network/allora-chain.git

cd allora-chain && make all

allorad version
```

## Add Wallet
* You can use your keplr seed-phrase to recover your wallet or create a new one
```console
# Recover your wallet with seed-phrase
allorad keys add testkey --recover

#OR

# Create a new wallet
allorad keys add testkey
```

## Get Faucet
> Connect to Allora [dashboard](https://app.allora.network?ref=eyJyZWZlcnJlcl9pZCI6IjVlNmEwMjc5LTcxNjEtNDhmYS04NGM3LWEzYzM0MGM4MGIzNyJ9) to find your Allora address
> 
> Get uAllo faucet [here](https://faucet.testnet-1.testnet.allora.network/)

![Screenshot_77](https://github.com/0xmoei/allora-testnet/assets/90371338/9e1d6236-ff51-48a1-a9f6-1149c842a4d0)

![Screenshot_76](https://github.com/0xmoei/allora-testnet/assets/90371338/ff27b97d-d04f-42c4-aa1b-3fb666874098)

#

## Install & Run Worker

### Optional: Only if you ran a node before
```console
cd $HOME && cd basic-coin-prediction-node
docker compose down -v
docker container prune

cd $HOME && rm -rf basic-coin-prediction-node
```

#

### 1- Clone worker
```console
cd $HOME
git clone https://github.com/allora-network/basic-coin-prediction-node
cd basic-coin-prediction-node
```

### 2- Config Worker
```console
# Remove config file
rm -rf config.json

# Create new config file
nano config.json
```

**Paste below code in it**
* Replace your wallet `Seed Phrase`
* `addressKeyName` was set as testkey since we choose it in step: Add Wallet
```
{
    "wallet": {
        "addressKeyName": "testkey",
        "addressRestoreMnemonic": "Seed Phrase",
        "alloraHomeDir": "",
        "gas": "1000000",
        "gasAdjustment": 1.0,
        "nodeRpc": "https://sentries-rpc.testnet-1.testnet.allora.network/",
        "maxRetries": 1,
        "delay": 1,
        "submitTx": false
    },
    "worker": [
        {
            "topicId": 1,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 2,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 7,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        }
    ]
}
```
Ctrl+X+Y+Enter to save & exit

### 3- Run Worker
```console
chmod +x init.config
./init.config
```
* If you need to make changes to your `config.json` , you must rerun this command again after your changes are done


```console
docker compose up -d --build
```

### 4- Check logs
Containers:
```console
docker compose ps
```

worker:
```console
docker compose logs -f worker
```
![image](https://github.com/user-attachments/assets/63ca0e84-c802-416a-a872-af6331aa776f)

inference:
```console
docker compose logs -f inference
```
![image](https://github.com/user-attachments/assets/a8133f85-b643-484d-beeb-cdfb51d6fd5c)

updater:
```console
docker compose logs -f updater

# Response:
# updater-basic-eth-pred  | UPDATING INFERENCE WORKER DATA
# updater-basic-eth-pred  | Response content is '0'
```

Check ETH price:
```console
curl http://localhost:8000/inference/ETH
```

# You are going to receive points in the [dashboard](https://app.allora.network/points/leaderboard) in the next few hours
