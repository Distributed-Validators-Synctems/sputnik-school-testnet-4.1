# Validators School Sputnik Testnet


## Sputnik app-chain binaries installation (sputnikd)

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
sputnikd genesis add-genesis-account <key-name> 10000000usputnik
```

### Create GenTX

```
# Create the gentx.
# Note, your gentx will be rejected if you use any amount greater than 10000000usputnik.
(Maybe app will ask to increase stake amount to 100000000usputnik)
# Make sure that all participants built their gentx files without typos.

sputnikd genesis gentx <key-name> 10000000usputnik \
  --pubkey=$(sputnikd tendermint show-validator) \
  --chain-id=<current course chain id> \
  --moniker="my-moniker" \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01"
```

### Add all accounts to genesis

```
# Add account addresses of all participants before generating genesis.
# (whose Gentx files you're using to generate genesis)
sputnikd genesis add-genesis-account <account-address> 10000000usputnik
```

### Generate genesis

```
sputnikd genesis collect-gentxs
```

### Start network

```
sputnikd start
```
