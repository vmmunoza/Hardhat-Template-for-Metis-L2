# Metis 101: Deploy and Explore

Deploy a smart contract on Metis using Hardhat. 
Hardhat is a development environment to compile, deploy, test, and debug smart contracts. 

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



