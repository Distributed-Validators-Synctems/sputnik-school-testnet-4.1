# Validators School Sputnik Testnet


## Sputnik app-chain binaries installation (sputnikd)

For the sake of simplicity we decided to use Cosmos Hub service binary. In order to install it please follow steps from this [instruction](https://hub.cosmos.network/main/getting-started/installation.html). It is based on the `v12.0.0` version of `sputnikd` binary.
Please check versiob of used bianry by running this command `sputnikd version --long`. You should get big list of text and at the beginig of it you should have following lines:
```
name: gaia
server_name: sputnikd
version: v12.0.0
commit: 6f8067d76ce30996f83645862153ccfaf5f13dd1
build_tags: netgo ledger
```

## GenTx generation

### Init
```bash:
sputnikd init "<moniker-name>" --chain-id <current course chain id>
```

### Generate keys

```bash:
# To create new keypair - make sure you save the mnemonics!
sputnikd keys add <key-name> 
```

or
```
# Restore existing odin wallet with mnemonic seed phrase. 
# You will be prompted to enter mnemonic seed. 
sputnikd keys add <key-name> --recover
```
or
```
# Add keys using ledger
sputnikd keys show <key-name> --ledger
```

Check your key:
```
# Query the keystore for your public address 
sputnikd keys show <key-name> -a
```

### Create account to genesis

```
sputnikd add-genesis-account <key-name> 1000000000uatom --keyring-backend <os | file>
```

### Create GenTX

```
# Create the gentx.
# Note, your gentx will be rejected if you use any amount greater than 1000000000uatom.
sputnikd gentx <key-name> 1000000000uatom --output-document=gentx.json \
  --chain-id=<current course chain id> \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --details=<your-node-details> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --keyring-backend <os | file>
```
