# Integrate Aave Protocol Data

The Aave Protocol is a decentralized non-custodial liquidity protocol where
users can participate as suppliers or borrowers. The protocol is a set of open
source smart contracts which facilitate the logic of user interactions. These
contracts, and all user transactions/balances are stored on a public ledger
called a blockchain, making them accessible to anyone.

This guide will explain how to integrate Aave protocol data for live and historical data.

1.  [Live Data](#live-data)
2.  [Historical Data](#transaction-methods)
    - a. [Events](#querying-events)
    - b. [Indexed Data](#indexed-data)

<br>

## Live Data

The best way to display live protocol data is to query directly from the blockchain. There are two view contracts which provide the methods to fetch the majority of relevant protocol data:

- `UiPoolDataProvider`: Used for querying reserve and user data
- `UiIncentiveDataProvider`: Used for querying reward emissions and user
  claimable rewards

These contracts can be queried using any blockchain library, or the [Aave Utilities](https://github.com/aave/aave-utilities) SDK is a wrapper for ethers.js which simplifies the process of fetching and formatting data from these contracts.

Examples of applications which integrate these contracts for live data include [Aave Interface](https://github.com/aave/interface) and [config.fyi](https://github.com/WeAreNewt/config.fyi).

<br>

## Historical Data

There are several methods to fetch historical market and user data. They are broken here into two categories: events and indexed data.

Querying events is a way to get historical data directly from the source contracts, however, this method is query-intensive and will require a customized script for each data point you want to collect. If you plan to index data yourself though this a good general and reliable approach.

Querying indexed data relies on secondary sources to index events and expose an endpoint to query. These solutions are typically easier to query but less flexible in the type of queries that are possible.

### Querying Events

Events are emitted by smart contracts to signify a state change. These events can be queried and pieced together to form a historical snapshot of the protocol. The general approach for querying events is to specific a contract address and abi (event schema) and listen for events between two block numbers. There are two examples below showing this process with two different data sources: Etherscan API and direct RPC calls.

For the Aave Protocol, the contract which emits most major events is the `Pool` contract. The address and abi for this contract across different markets can be found in the [deployed contracts](https://docs.aave.com/developers/deployed-contracts/deployed-contracts).

<details>
    <summary>Parse Etherscan Events</summary>

```js
const ethers = require("ethers");
const request = require("request-promise-native");

// REPLACE WITH YOUR DATA HERE
const abi = require("./yourAbi.json");
const contractAddress = "0x0";
const contractStartBlock = 1; // deployment block of the contract, to avoid filtering from genesis block
const ETHERSCAN_API_KEY = "";

async function fetchEvents() {
  // Set up the API key and base URL for the Etherscan API
  const baseUrl = "https://api.etherscan.io/api";

  const contractInterface = new ethers.utils.Interface(abi);

  // Set the initial page and block number
  let page = 1;
  let blockNumber = "latest";
  const PAGE_SIZE = 1000;

  // Loop through each transaction
  while (true) {
    // Set up the options for the API request
    const options = {
      url: `${baseUrl}`,
      qs: {
        module: "account",
        action: "txlist",
        address: contractAddress,
        startblock: contractStartBlock,
        endblock: blockNumber,
        page: page,
        sort: "asc",
        apikey: ETHERSCAN_API_KEY,
      },
      json: true,
    };

    // Send the API request
    const response = await request(options);

    for (const tx of response.result) {
      // Check if this transaction was sent to the contract
      if (tx.to.toLowerCase() === contractAddress.toLowerCase()) {
        // Add the transaction to the array
        const decodedData = contractInterface.decodeFunctionData(
          tx.input.slice(0, 10),
          tx.input
        );
        // Use decoded data here
      }
    }
    if (response.result.length < PAGE_SIZE) {
      break;
    } else {
      page += 1;
    }
  }
}

fetchEvents();
```

</details>

<details>
    <summary>Parse RPC Events</summary>

Note: An archival node may be necessary for the rpcUrl to query historical events

```js
const { providers, Contract, utils } = require("ethers");

// REPLACE WITH YOUR DATA HERE
const abi = require("./yourAbi.json");
const contractAddress = "0x0";
const contractStartBlock = 1; // deployment block of the contract, to avoid filtering from genesis block
const rpcUrl = "https://eth-mainnet.public.blastapi.io"; // eth mainnet example

async function fetchEvents() {
  // Connect to an Ethereum node
  const provider = new providers.JsonRpcProvider("rpcUrk");

  // Create a Contract object
  const contract = new Contract(contractAddress, abi, provider);

  // Define the filter options
  const filter = {
    fromBlock: 0,
    toBlock: "latest",
    address: contractAddress,
  };

  // Get the events from the contract
  const events = await contract.provider.getLogs(filter);

  // Loop through the events and log them
  for (const event of events) {
    const eventJson = utils.parseLog(event);
    console.log(eventJson);
  }
}

fetchEvents();
```

</details>

### Query Indexed Data

There are also services which expose endpoints for indexed data. Examples of this for Aave Protocol data are:

- [Aave Protocol Subgraphs](https://github.com/aave/protocol-subgraphs)
- [Dune Analytics](https://dune.com/projects/aave)
- [Covalent](https://www.covalenthq.com/docs/unified-api/class-c/aave/)

The Graph is most common for application integrations and has a [querying guide](https://thegraph.com/docs/en/querying/querying-the-graph/) with tips for integrating GraphQL endpoints. There are also sample queries such as [user transaction history](https://github.com/aave/protocol-subgraphs#user-transaction-history) in the protocol subgraphs README.
