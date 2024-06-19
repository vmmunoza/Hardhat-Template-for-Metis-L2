# Metis 101: Deploy and Explore

Deploy your first smart contract on Metis L2 using Hardhat. 


### Setting Up the Project Environment

1. **Create a Project Directory:**
   - **Command:** `mkdir new_project`
   - **Purpose:** This command creates a new folder named `new_project` for your project files.
     
2. **Initialize a Node.js Project:**
   - **Command:** `npm init`
   - **Purpose:** Initializes a new Node.js project. This command creates a `package.json` file in your project directory, which holds various metadata relevant to the project. This file is used to manage the project’s dependencies, scripts, and versioning.

3. **Install Hardhat:**
   - **Command:** `npm install --save-dev hardhat`
   - **Purpose:** Installs Hardhat as a development dependency in your project. Hardhat is a task runner and testing framework designed for Ethereum development.

### Setting Up Hardhat

4. **Create a Hardhat Project:**
   - **Command:** `npx hardhat`
   - **Purpose:** Initializes a new Hardhat project. This command will prompt you to create a basic project structure and install necessary dependencies.

5. **Select Default Options:**
   - When asked to install the sample project's dependencies, choose `yes`. This action installs the `@nomicfoundation/hardhat-toolbox`, a set of tools and plugins recommended for most Hardhat projects.

### Configuring Hardhat for Metis Deployment

6. **Modify the Hardhat Configuration:**
   - **File:** `hardhat.config.js`
   - **Purpose:** To deploy contracts on the Metis network, you need to specify the Metis network configuration in the Hardhat config file. This involves setting up the network name, RPC URL, accounts (private keys), and any additional network-specific settings.

7. **Network Configuration Sample:**
   ```javascript
   module.exports = {
     solidity: "0.8.24",
     networks: {
       hardhat: {},
       metisAndromeda: {
         url: "https://andromeda.metis.io/?owner=1088",
         accounts: [PRIVATE_KEY],
       },
     },
   };
   ```
   - **Explanation:** This configuration sets the Solidity compiler version and defines a custom network (`metisAndromeda`). The `url` is the RPC endpoint for the Metis Andromeda network, and `accounts` should include the private key(s) for the account(s) you will use to deploy the contract.

8. **Setting the Private Key:**
   - **Best Practice:** Instead of hardcoding the private key in the config file, use an environment variable to enhance security. You can use `dotenv` package for managing environment variables.
   - **Change:** `accounts: [process.env.WALLET_PRIVATE_KEY]` to use an environment variable.
   - **Adding Private Key to Config:**
     ```javascript
     require('dotenv').config();
     const PRIVATE_KEY = process.env.WALLET_PRIVATE_KEY;
     ```
   - **Purpose:** Safely loads your private key from an environment variable, reducing the risk of exposing sensitive information.

### Deploying the Contract

9. **Deploy Your Contract:**
   - **Command:** `npx hardhat run .\scripts\deploy.js --network metisAndromeda`
   - **Purpose:** This command compiles your smart contracts and deploys them to the specified network (in this case, `metisAndromeda`). The script `deploy.js` contains your deployment logic.

10. **Verification and Interaction:**
    - After deployment, you will receive a transaction hash. You can use this hash to verify your contract's deployment on the Metis blockchain explorer.
    - Interacting with your deployed contract can be done through Hardhat scripts or directly via a web3 provider interface like Metamask.
______

### Lock.sol Contract Overview

- **Purpose:** The `Lock` contract is designed to lock Ether (or Metis in our case) until a specified future time. Once the time lock expires, the locked funds can be withdrawn by the contract's owner.

### Key Components

1. **State Variables:**
   - `unlockTime`: A `public` uint variable that stores the timestamp (in seconds since the Unix epoch) until which the funds are locked.
   - `owner`: A `public` and `payable` address variable that stores the contract owner's address, who is allowed to withdraw the funds after the unlock time.

2. **Event:**
   - `Withdrawal`: An event that logs the withdrawal action, including the amount of ETH withdrawn and the timestamp of the withdrawal.

3. **Constructor:**
   - The constructor takes a single parameter `_unlockTime` (the future timestamp until which the funds should be locked) and sets the contract's `unlockTime` and `owner` state variables. It requires that the `_unlockTime` is in the future compared to the current `block.timestamp`.
   - The constructor also allows the deployment transaction to include ETH, making the contract's address payable and enabling it to hold and lock the transferred ETH.

4. **Functions:**
   - `withdraw()`: This function allows the contract owner to withdraw all the locked funds after the `unlockTime` has passed. It includes two `require` statements to ensure that:
     1. The current block timestamp is equal to or later than the `unlockTime`, ensuring the funds are locked until the specified time.
     2. The caller of the function (`msg.sender`) is the owner of the contract, preventing unauthorized access to the funds.
   - Upon successful validation, the `Withdrawal` event is emitted, and the contract transfers its entire balance to the owner.

### Logic and Expectations

- **Locking Mechanism:** Upon deployment, the contract locks any ETH sent with the constructor transaction until the specified `unlockTime`.
- **Security:** The contract ensures that only the owner can withdraw the funds and only after the specified unlock time.
- **Transparency:** The `unlockTime` and `owner` are publicly visible, making the contract's state and intentions transparent.
- **Event Logging:** The `Withdrawal` event provides an auditable log of withdrawals, useful for tracking when and how funds are withdrawn.

### Interaction
You can interact with the `Lock.sol` contract once it's deployed on the Metis network. Interacting with a smart contract typically involves two types of operations: **read operations**, which don't change the state of the blockchain and therefore don't require a transaction fee, and **write operations**, which do change the state and require gas fees. For the `Lock.sol` contract, you'll mainly be concerned with a write operation (`withdraw`) and reading state variables (`unlockTime` and `owner`).

______
#### (Incomplete) List of useful resources for building on Metis

- Documentation for devs
https://docs.metis.io/dev/

- Gitbook
https://github.com/Metis-DAO/Metis

- Hardhat and Typescript tutorial
https://github.com/harshnambiar/namaste_metis

- Subgraphs
https://docs.ormi.xyz/metis-subgraphs/

- The public Metis endpoint on Nodies:
https://lb.nodies.app/v1/f5c5ecde09414b3384842a8740a8c998

**Note:** In case when you need higher limits of the public RPC endpoint, there are two options:
1. Setting up a private RPC endpoint using a provider such as Nodies or BlastAPI
2. Setting up a replica node. 

Run Replica Node here: https://github.com/MetisProtocol/metis-replica-node



