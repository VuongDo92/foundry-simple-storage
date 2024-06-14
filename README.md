## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```

## Deploy by unsafety env private key and rpc url
This way is not good for real product but is so easy for development. You should be carefull while using it.

Steps by steps for run env value
1. Create a file named `.env`, then do not forget ignore that file in gitignore file

2. Run anvil shell, then copy a private key and rpc url to `.env` file
```shell
$ anvil
```

```environment value in file .env
PRIVATE_KEY=...
RPC_URL=...
```

2. Open another shell, then run a below shell to have private key and rpc url as a environment value
```shell
$ source .env
```

3. Deploy your contract with a below CLI
```shell
$ forge script script/DeploySimpleStorage.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

### For the moment, a `$PRIVATE_KEY` in the `.env` file is cool as long as we do not expect the file.
### But for the real money, I will not do that. I will use `--interactive` or a keystore file with a password once foundry adds that

Using ERC-2335 to encrypted privatekey to a json format

1. To create a default local wallet, run this:
```shell
$ cast wallet import defaultWallet --interactive 
# Enter private key:
# Enter password:
# `defaultKey` keystore was saved successfully. Address: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
```
2. Applying it to deploy a smart contract. We can save password on a file named `.password` which is unforgetablelly ignored in the ignore file. PLEASE DO NOT LEAK ITS PASSWORD. 
```shell
$ forge script script/DeploySimpleStorage.s.sol --rpc_url $RPC_URL --account defaultkey --sender 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 --password-file .password --broadcase -vvvv
$ Enter Keystore Password:
```

3. To see wallet list on your local, run this:
```shell
$ cast wallet list
```

4. To see Json Format of the default wallet created, follow this set of CLI:
```shell
$ cd ~ # To go to root folder
$ cd .foundry/keystores/ # To go to folder keystore
$ ls # To see all keystores
$ cat defaultKey # To see json format that is encripted by ERC-2335 standard
```

### To clear on history CLI written, please run these set of CLI:
```shell
$ history # To see all CLI that you had written
$ history -c # To clear them
$ rm .bash_history
```

### Interact with the contract with CLI:
After deploying contract succcessful, we take a contract address and deal with contract interactive
For the example, the contract address is `0x5FbDB2315678afecb367f032d93F642f64180aa3` which is just deployed successful on the previous steps
```shell
$ cast --help
$ cast send --help # to call a transak (yellow) function on your smart contract
$ cast send 0x5FbDB2315678afecb367f032d93F642f64180aa3 "store(uint256)" 123 --rpc-url $RPC_URL --private-key $PRIVATE
$ Call store function this is actually similar with a store yellow function on remix with paramater is 123, paramater type is uint256 and function name is store
$ cast call --help # to read a readable/viewable (green) function on your smart contract
$ cast call 0x5FbDB2315678afecb367f032d93F642f64180aa3 "retrieve()" # we will see a hash value back then
$ cast --to-base `a hash value from a result of the last step` dec # we will able to see a result value as a decimal number
```

### Deploying a smart contract on testnet (Sepolia)
Deploying a smart contract to Sepolia, firstly you need small amount of eth (0.0001eth) in your wallet.
Then use https://alchemy.com to help you deposit a fake amount of eth to your wallet on Sepolia Test Net. 
PRIORITY: Your wallet have to connect to Alchemy site before asking them to deposit FAKE ETH for you

To deploy a smart contract, you need to follow a bunch of below steps:
```shell
$ forge script script/DeploySimpleStorage.s.sol --rpc-url $SEPOLIA_RPC_URL --account defaultKey --sender 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 --password-file .password --broadcast -vvvv 
```