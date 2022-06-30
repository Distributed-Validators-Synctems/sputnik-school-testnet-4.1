# Validators School testnet


## Cosmos Hub binaries installation (gaiad)

For the sake of simplicity we decided to use Cosmos Hub service binary. In order to install it please follow steps from this [instruction](https://hub.cosmos.network/main/getting-started/installation.html). It is based on the `v7.0.2` version of `gaiad` binary.
Please check versiob of used bianry by running this command `gaiad version --long`. You should get big list of text and at the beginig of it you should have following lines:
```
name: gaia
server_name: gaiad
version: v7.0.2
commit: cd27aaaf39cc7819b5164e4baf3fd5aad23ec52a
build_tags: netgo ledger
```

## GenTx generation

### Init
```bash:
gaiad init "<moniker-name>" --chain-id <current course chain id>
```

### Generate keys

```bash:
# To create new keypair - make sure you save the mnemonics!
gaiad keys add <key-name> 
```

or
```
# Restore existing odin wallet with mnemonic seed phrase. 
# You will be prompted to enter mnemonic seed. 
gaiad keys add <key-name> --recover
```
or
```
# Add keys using ledger
gaiad keys show <key-name> --ledger
```

Check your key:
```
# Query the keystore for your public address 
gaiad keys show <key-name> -a
```

### Create account to genesis

```
gaiad add-genesis-account <key-name> 1000000000uatom --keyring-backend <os | file>
```

### Create GenTX

```
# Create the gentx.
# Note, your gentx will be rejected if you use any amount greater than 1000000000uatom.
gaiad gentx <key-name> 1000000000uatom --output-document=gentx.json \
  --chain-id=dvs-protocol-v1 \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --details=<your-node-details> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --keyring-backend <os | file>
```
