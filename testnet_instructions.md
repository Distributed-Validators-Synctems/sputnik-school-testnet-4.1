# Distributed **Validators School Testnet**

## **Quick Links**

Genesis: `https://raw.githubusercontent.com/Distributed-Validators-Synctems/sputnik-school-testnet/master/genesis.json`

Block explorer: https://sputnik-testnet-explorer.nodejumper.io/sputnik-school-testnet/staking

Peers: `745870cb265d8c3785da62317d46ecfaf7afa20a@95.217.164.96:26656,db0b3f18280de5ef4cb294912df6a3bf2c1a3f49@95.214.53.164:26656,7413a980948bf2208876d8bb40bb76ea957b1d97@178.215.71.199:26656,d236c62edc593476ad79bb13b028ab80e1c15d3a@128.199.142.104:26656,af6a8ce05b07dadf452fde8c645814c579d06142@88.218.60.250:26656`

Chain Id: `sputnik-practice-1`

## **Hardware Requirements**

Here are the minimal hardware configs required for running a validator/sentry node

- 4GB RAM
- 2vCPUs
- 80GB Disk space

## **Software Requirements**

- Ubuntu 22.04+
- [Go v1.21](https://golang.org/doc/install)

## **Install sputnikd, Generate Wallet and Submit GenTx**

### ****Sputnik app-chain binaries installation (sputnikd)****

Sputnik app testnet binary repo
https://github.com/Distributed-Validators-Synctems/sputnik-app-chain-practice

```
commit: d3ed2906478c1558e4be1a2e0f98305f7be46832
cosmos_sdk_version: v0.47.13-ics-lsm
go: go version go1.21.9 linux/amd64
name: sputnik
server_name: sputnik
version: main-d3ed2906478c1558e4be1a2e0f98305f7be46832
```

### Network init

```bash
cd ~
sputnikd init "<moniker-name>" --chain-id sputnik-practice-1
```

example:

```bash
sputnikd init course-participant-1 --chain-id dvs-course-testnet
```

### **Create Validator Key**

It's very important that after you run this command that you save the seed phrase that is generated. If you do not save you phrase, you will not be able to recover this account.

`sputnikd keys add <your validator key name>`

or restore existing wallet with mnemonic seed phrase. You will be prompted to enter mnemonic seed.

`sputnikd keys add <key-name> --recover`

or add keys using ledger

`sputnikd keys add <key-name> --ledger`

Check your key:

`sputnikd keys show <key-name> -a`

### ****Create account to genesis****

This command will help you to create account in your local genesis file. It will add funds to your address. Otherwise `sputnikd getntx` command will fail because of lack of funds.

`sputnikd add-genesis-account <key-name> 1000000000usputnik`

### ****Create GenTX****

Create the gentx file. Note, your gentx will be rejected if you use any amount greater than 10000000usputnik.

```
sputnikd gentx <key-name> 10000000usputnik \
  --chain-id=sputnik-practice-1 \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --details=<your-node-details> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01"
```

After gentx will be ready you can find it in the `~/.sputnik/config/gentx` directory. After that you will be required to upload it into `gentxs` directory of this repository. Please name it using following template `gentx-<validator name>.json`.

In order to upload this file you will need to create fork of this repository. Please click on “Fork” button in the top right corner of this page, and name it somehow or leave repository name unchanged.
After that you can upload `gentx` file into appropriate directory of your repository. Next, you will need to create a PR (Pull request) to add changes from your cloned repository into main repository.
Please go into root directory of your repository and click on “Contribute” button.
You will see this popup window.

Please “Open pull request”, check data, put some description into text box field and click on “Create pull request” inside it. Congratulations you have created your first pull request!

### Create validator after genesis

```
sputnikd tx staking create-validator \
  --amount=10000000usputnik \
  --pubkey=$(sputnikd tendermint show-validator) \
  --chain-id=sputnik-practice-1 \
  --moniker="<moniker-name>" \
  --website="<your-node-website>"\
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --gas="auto" \
  --gas-adjustment=1.3 \
  --gas-prices="0.1usputnik" \
  --from=<key_name>
```

## Run node

Install `curl`
```
sudo apt install curl -y
```

### ****Download genesis****

To download `genesis.json` file
```
curl https://raw.githubusercontent.com/Distributed-Validators-Synctems/sputnik-school-testnet/master/genesis.json > ~/.sputnik/config/genesis.json
```
After downloading you need to verify your `genesis.json` checksum

```
sha256sum ~/.sputnik/config/genesis.json
```

you should see `d416341bd5dd384bb78dc43c0a4413014827c54e82989df9fcc28d3d5df5e8ab` in the output.

Set the peers
```bash
PEERS="745870cb265d8c3785da62317d46ecfaf7afa20a@95.217.164.96:26656,db0b3f18280de5ef4cb294912df6a3bf2c1a3f49@95.214.53.164:26656,7413a980948bf2208876d8bb40bb76ea957b1d97@178.215.71.199:26656,d236c62edc593476ad79bb13b028ab80e1c15d3a@128.199.142.104:26656,af6a8ce05b07dadf452fde8c645814c579d06142@88.218.60.250:26656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.sputnik/config/config.toml
```

Set `minimum-gas-prices` to `.sputnik/config/app.toml`
```bash
MIN_GAS_PRICES=0.001usputnik
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "'$MIN_GAS_PRICES'"|' $HOME/.sputnik/config/app.toml
```

### ****Set Up Cosmovisor****

Set up cosmovisor to ensure any future upgrades happen flawlessly. To install Cosmovisor
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```

Create the required directories and files
```
mkdir -p ~/.sputnik/cosmovisor/genesis/bin
mkdir -p ~/.sputnik/cosmovisor/upgrades
```

After directories will be ready please copy `sputnikd` binaries created in the “Cosmos Hub binaries installation (sputnikd)” section into `~/.sputnikd/cosmovisor/genesis/bin` directory. You can do it using next command
```
cp ~/go/bin/sputnikd ~/.sputnik/cosmovisor/genesis/bin/sputnikd
```

### ****Set Up sputnikd Service****

Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:
```
sudo tee /etc/systemd/system/sputnikd.service > /dev/null << EOF
[Unit]
Description=Sputnik app chain daemon
After=network-online.target
[Service]
Environment="DAEMON_NAME=sputnikd"
Environment="DAEMON_HOME=${HOME}/.sputnik"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
Environment="UNSAFE_SKIP_BACKUP=true"
User=$USER
ExecStart=${HOME}/go/bin/cosmovisor run start
Restart=always
RestartSec=3
LimitNOFILE=infinity
LimitNPROC=infinity
[Install]
WantedBy=multi-user.target
EOF
```

And start service:
```
sudo systemctl daemon-reload
sudo systemctl enable sputnikd 
sudo systemctl restart sputnikd
```

How you can check the logs
```
sudo journalctl -u sputnikd -f
```

Set chain-id to `sputnik-practice-1` (for CLI)
```
sputnikd config chain-id sputnik-practice-1
```

## **More about validators**

Please refer to the Cosmos Hub documentation on validators for a general overview of running a validator. We are using the exact same validator model and software, but with slightly different parameters and other functionality specific to the Cosmic Horizon Network.

- [Run a Validator](https://hub.cosmos.network/validators/validator-setup)
- [Validators Overview](https://hub.cosmos.network/validators/overview)
- [Validator Security](https://hub.cosmos.network/validators/security)
- [Validator FAQ](https://hub.cosmos.network/validators/validator-faq)
