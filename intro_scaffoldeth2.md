METIS SCAFFOLD WORKSHOP WALKTHROUGH


**need yarn, recommend install globally | Node.js version >= v18.17.0 is required.


Download scaffold boilerplate, open in VSCode

Run command “yard scaffold” to start

Name project “demo” 

Use hardhat framework

Yes install

****will take a minute to install everything***

CD into your project name “cd demo” 

Run Yarn start


Change .env.example to .env and enter private key for wallet with gas tokens

Add network info to hardhat.config.ts 


```
  metisAndromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
      accounts: [process.env.WALLET_PRIVATE_KEY],
      verify: {
        etherscan: {
          apiKey: "apiKey is not required, just set a placeholder",
          apiUrl:
            "https://api.routescan.io/v2/network/mainnet/evm/1088/etherscan",
        },
      },
},
````

To start VM, run “yarn chain”

To deploy run “Yard deploy —network metisAndromeda”





4 things to setup:

(1)hardhatconfig.ts

—> add metisandromeda line53


—> env.example —> .env add WALLET PRIVATE KEY=0x+private key

deployyourcontract.ts

Remove greeting line / console log line 37 by Changing to —> yourContract.totalSupply or commenting out 

——>Line 28 add args “Metis” “test” “2000” // add 18 zeros to whatever token amount you want 

Lines 25, 36, 44, change YourContract to contract name (ie. FixedToken)

Cd packages

Cd hardhat

Run “yarn compile” (not necessary but good step to show for developer understanding)

Deploy with: 

npx hardhat deploy --network metisAndromeda

verify with: 

yarn hardhat --network metisAndromeda etherscan-verify



Get out of hardhat / packages cd 

Yarn start in main directory to launch scaffold front end

To make changes - 

scaffold.config.ts - connect smart contract to scaffold front-end 

Line 14 chains.hardhat —> chains.metis 

Contract will now be connected to scaffold in local host
