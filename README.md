# 🍣 sashimi
Minimal docker setup to run a testnet node with lnd and taro.


## Setup
 
```
git clone https://github.com/davisv7/sashimi
cd sashimi
docker-compose up -d
```

When you run this command for the first time, LND will first synchronise with the Bitcoin blockchain. It can take a
long time and tapd won't start before LND is done. Tapd will be ready when the message `Taproot Asset Daemon fully
active!` is displayed in the logs.

## Creating an address via Docker

First, get the docker container ID of the lnd and tapd with: `docker ps`

Then, create a wallet with: `docker exec -it -u lnd <LND Container Id> lncli --network=testnet create` (password :
C4-t]-#6uV{BVPQ~).

Create an address with: `docker exec -it -u lnd <LND Container Id> lncli --network=testnet newaddress p2wkh`

If you are running on testnet, you now need to get some testnet bitcoin [here](https://bitcoinfaucet.uo1.net).

## Creating an asset

We run this command (it doesn't create the asset right away as you can add several and do it in only one transaction).

```bash
docker exec -it -u tap <TAPD Container Id> \
                tapcli assets mint \
                --type normal \
                --name myRoylloCoin \
                --supply 999 \
                --meta_bytes "Used by Royllo"
```

The output is something like this:

```
{
    "batch_key": "025720830b02a85857ffbc827ad7ae5cef404b45d7c5e0b9015441d0bf033ba7a1"
}

```

You can create the asset with the batch key with this command:

```bash
docker exec -it -u tap <TAPD Container Id> \
              tapcli assets mint finalize
```

You can then see your asset here :
    
```bash
docker exec -it -u tap <TAPD Container Id> \
              tapcli assets list
```



Thanks:

https://hub.docker.com/r/polarlightning

https://github.com/royllo/lnd-taro-with-docker

https://github.com/vulpemventures/nigiri

https://github.com/straumat
