# Worker with 9 topics using Huggingface model

## Stop old nodes
* If you are running basic-price node, stop it
```console
cd $HOME && cd basic-coin-prediction-node
docker compose down -v
docker container prune
```

## Config HuggingFace worker
```console
cd $HOME
git clone https://github.com/allora-network/allora-huggingface-walkthrough
cd allora-huggingface-walkthrough
```
```console
mkdir -p worker-data
chmod -R 777 worker-data
```
```
cp config.example.json config.json
nano config.json
```

Paste below code:
* Replace `testkey` with wallet name: `allorad keys list`
* Replace `SeedPhrase` with your wallet seed phrase
* `nodeRpc`: You can use `https://allora-rpc.testnet-1.testnet.allora.network/` or `https://sentries-rpc.testnet-1.testnet.allora.network/`
```
{
    "wallet": {
        "addressKeyName": "testkey",
        "addressRestoreMnemonic": "SeedPhrase",
        "alloraHomeDir": "/root/.allorad",
        "gas": "1000000",
        "gasAdjustment": 1.0,
        "nodeRpc": "https://allora-rpc.testnet-1.testnet.allora.network/",
        "maxRetries": 1,
        "delay": 1,
        "submitTx": false
    },
    "worker": [
        {
            "topicId": 1,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 1,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 2,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 3,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 3,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BTC"
            }
        },
        {
            "topicId": 4,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 2,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BTC"
            }
        },
        {
            "topicId": 5,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 4,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "SOL"
            }
        },
        {
            "topicId": 6,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "SOL"
            }
        },
        {
            "topicId": 7,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 2,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ETH"
            }
        },
        {
            "topicId": 8,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 3,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "BNB"
            }
        },
        {
            "topicId": 9,
            "inferenceEntrypointName": "api-worker-reputer",
            "loopSeconds": 5,
            "parameters": {
                "InferenceEndpoint": "http://inference:8000/inference/{Token}",
                "Token": "ARB"
            }
        }
        
    ]
}
```

CTRL+X+Y+ENTER to save and exit

## Create Coingecko API key
https://www.coingecko.com/en/developers/dashboard

* Replace Coingecko API in `app.py`

```
nano app.py
```

![Screenshot_186](https://github.com/user-attachments/assets/ad0c0192-4fb0-4708-9378-443e5adb1928)
CTRL+X+Y+Enter to save & exit

## Run Huggingface Worker
```console
chmod +x init.config
./init.config
```

```console
docker compose up --build -d
```

Logs:
```
docker compose logs -f worker
```
```
docker compose logs -f
```
