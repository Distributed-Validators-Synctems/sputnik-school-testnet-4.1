# Distributed **Validators School Testnet**

## **Quick Links**

Genesis: TBD

Block explorer: TBD

Peers: TBD

Chain Id: `sputnik-practice-1`

## **Hardware Requirements**

Here are the minimal hardware configs required for running a validator/sentry node

- 4GB RAM
- 2vCPUs
- 80GB Disk space

## **Software Requirements**

- Ubuntu 22.04+
- [Go v1.21+](https://golang.org/doc/install)

## **Install sputnikd, Generate Wallet and Submit GenTx**

### ****Sputnik app-chain binaries installation (sputnikd)****

For the sake of simplicity we decided to use Cosmos Hub service binary. In order to install it please follow steps from official Cosmos HUB [instructions](https://hub.cosmos.network/main/getting-started/installation.html). It is based on the `v12.0.0` version of `sputnikd` binary. Please check version of used binary by running this command `sputnikd version --long`. You should get big list of text and at the beginning of it you should have the following lines:

```
name: gaia
server_name: sputnikd
version: v12.0.0
commit: 6f8067d76ce30996f83645862153ccfaf5f13dd1
build_tags: netgo ledger
```

### Network init

```bash
cd ~
sputnikd init "<moniker-name>" --chain-id school-testnet-4
```

example:

```bash
sputnikd init course-participant-1 --chain-id dvs-course-testnet-4
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

`sputnikd add-genesis-account <key-name> 1000000000uatom`

### ****Create GenTX****

Create the gentx file. Note, your gentx will be rejected if you use any amount greater than 1000000000uatom.

```
sputnikd gentx <key-name> 1000000000uatom \
  --chain-id=school-testnet-4 \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --details=<your-node-details> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01"
```

After gentx will be ready you can find it in the `~/.sputnikd/config/gentx` directory. After that you will be required to upload it into `gentxs` directory of this repository. Please name it using following template `gentx-<validator name>.json`.

In order to upload this file you will need to create fork of this repository. Please click on “Fork” button in the top right corner of this page, and name it somehow or leave repository name unchanged.

![fork.png](https://raw.githubusercontent.com/kuraassh/school-testnet/master/fork.png)

After that you can upload `gentx` file into appropriate directory of your repository. Next, you will need to create a PR (Pull request) to add changes from your cloned repository into main repository.

Please go into root directory of your repository and click on “Contribute” button.

![contribute.png](https://raw.githubusercontent.com/kuraassh/school-testnet/master/contribute.png)

You will see this popup window.

![popup.png](https://raw.githubusercontent.com/kuraassh/school-testnet/master/popup.png)

Please “Open pull request”, check data, put some description into text box field and click on “Create pull request” inside it. Congratulations you have created your first pull request!

### Create validator after genesis

```
sputnikd tx staking create-validator \
  --amount=1000000000uatom \
  --pubkey=$(sputnikd tendermint show-validator) \
  --chain-id=school-testnet-4 \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --gas="auto" \
  --gas-adjustment=1.3 \
  --gas-prices="0.1uatom" \
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
curl https://raw.githubusercontent.com/Distributed-Validators-Synctems/school-testnet-4/master/genesis.json > ~/.gaia/config/genesis.json
```
After downloading you need to verify your `genesis.json` checksum

```
sha256sum ~/.gaia/config/genesis.json
```

you should see `16f4193398f60a06925a35afe89a164b374634a7397c198a3bb55133cdb5fbea` in the output.

Set the peers
```bash
PEERS="2cdad1c9cd0cdffd180695179928e57f4bfbe519@65.109.229.209:26656,0aaece51c70f806d5d3cdb85371ab2547ada951e@46.8.19.124:27656,ac1011c6c16cbd2cc8740e302781070da040cc4f@81.0.218.67:26656,4a52836783f21058a09082c9179d1bbe2dddb1e7@185.103.132.18:26656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.gaia/config/config.toml
```

### ****Set Up Cosmovisor****

Set up cosmovisor to ensure any future upgrades happen flawlessly. To install Cosmovisor
```
go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
```

Create the required directories and files
```
mkdir -p ~/.gaia/cosmovisor/genesis/bin
mkdir -p ~/.gaia/cosmovisor/upgrades
echo "" | sed 's/.*/{}/' > ~/.gaia/cosmovisor/genesis/upgrade-info.json
```

After directories will be ready please copy `sputnikd` binaries created in the “Cosmos Hub binaries installation (sputnikd)” section into `~/.sputnikd/cosmovisor/genesis/bin` directory. You can do it using next command
```
cp ~/go/bin/sputnikd ~/.gaia/cosmovisor/genesis/bin/sputnikd
```

### ****Set Up sputnikd Service****

Set up a service to allow cosmovisor to run in the background as well as restart automatically if it runs into any problems:
```
echo "[Unit]
Description=Cosmos Hub daemon
After=network-online.target
[Service]
Environment="DAEMON_NAME=sputnikd"
Environment="DAEMON_HOME=${HOME}/.gaia"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
Environment="UNSAFE_SKIP_BACKUP=true"
User=$USER
ExecStart=${HOME}/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=infinity
LimitNPROC=infinity
[Install]
WantedBy=multi-user.target
" >cosmovisor.service
```

Move this new file to the systemd directory:
```
sudo mv cosmovisor.service /lib/systemd/system/sputnikd.service
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

Set chain-id to `school-testnet-4` (for CLI)
```
sputnikd config chain-id school-testnet-4
```

## **More about validators**

Please refer to the Cosmos Hub documentation on validators for a general overview of running a validator. We are using the exact same validator model and software, but with slightly different parameters and other functionality specific to the Cosmic Horizon Network.

- [Run a Validator](https://hub.cosmos.network/main/validators/validator-setup.html)
- [Validators Overview](https://hub.cosmos.network/main/validators/overview.html)
- [Validator Security](https://hub.cosmos.network/main/validators/security.html)
- [Validator FAQ](https://hub.cosmos.network/main/validators/validator-faq.html)
