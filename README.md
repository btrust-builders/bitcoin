# Bitcoin Core Daemon
This setup document provides instructions on how to set up reusable bash scripts for easily running Bitcoin Core on different networks: **regtest, signet, testnet, and mainnet**.

## Software Requirements

To get started:

1. Clone this repository or the [latest bitcoin sourcecode](https://github.com/bitcoin/bitcoin):  
   ```bash
   git clone <repo-url>
   ```
2. Check the [doc folder](/doc) for specific build requirements for your operating system (Linux, macOS, Windows).


### Development Process

#### Validate Installation
To validate the Bitcoin Core installation, run:
```bash
bitcoind -daemon
```
This should output `Bitcoin Core starting`. To stop the daemon, run:
```bash
bitcoin-cli stop
```

#### Run Bitcoin Daemon (bitcoind)
1. Create a parent folder called `bitcoin-scripts` and a child folder inside called `datadir`:
   ```bash
   mkdir -p bitcoin-scripts/datadir
   ```
    Replace `<PATH_TO_DATA_DIR>` in the scripts below with the absolute path to the `datadir` folder.

2. Create the following scripts inside the `bitcoin-scripts` folder:

    #### **Create Startup Script for Regtest**

   **btc-start-regtest-node.sh**:
   ```bash
   # Start a Bitcoin Core node in regtest mode
   #!/bin/bash
   bitcoind -regtest -printtoconsole -server -rpcuser=user -rpcpassword=password -rpcport=18332 -txindex=1 -debug=net -datadir=<PATH_TO_DATA_DIR>
   ```

    #### **Create Command Wrapper for Regtest**
   **btc-regtest-cmd.sh**:
   ```bash
   # Run Bitcoin CLI commands in regtest mode
   #!/bin/bash
   bitcoin-cli -regtest --rpcuser=user --rpcpassword=password --rpcport=18332 $@
   ```

3. Make the scripts executable:
   ```bash
   chmod +x btc-start-regtest-node.sh
   chmod +x btc-regtest-cmd.sh
   ```

4. Start the Bitcoin node:
   ```bash
   ./btc-start-regtest-node.sh
   ```

5. Open another terminal to run some test cli commands:
   ```bash
   ./btc-regtest-cmd.sh generate 1     # Generate 1 block, should return the block hash.
   ./btc-regtest-cmd.sh getblockcount  # Get the current block count, should return a number
   ```

You can go through same steps by changing the network name on the bash file for your preferred network.

### Additional Resources
- [Bitcoin Core Documentation](https://developer.bitcoin.org/)
- [Learning Bitcoin from the Command Line](https://github.com/BlockchainCommons/Learning-Bitcoin-from-the-Command-Line)
- [Bitcoin Core RPC Documentation](https://developer.bitcoin.org/reference/rpc/)

### Troubleshooting
- **Port Conflicts**: Ensure the `rpcport` (e.g., `18332`) is not already in use.
- **Permission Errors**: Ensure the scripts are executable (`chmod +x`).
- **Incorrect Paths**: Double-check the `datadir` path in the scripts.
