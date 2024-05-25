# ethers cheat sheet
- https://www.npmjs.com/package/ethers

<br><br>

# Docs
- https://docs.ethers.org



<br><br>
<br><br>
_____________________________________________
_____________________________________________
<br><br>
<br><br>

# Connecting to Ethereum
- https://docs.ethers.org/v6/getting-started/#starting-connecting
- The quickest and easiest way to experiment and begin developing on Ethereum is to use MetaMask, which is a browser extension that injects objects into the window, providing:

  - read-only access to the Ethereum network (a Provider)
  - authenticated write access backed by a private key (a Signer) 

When requesting access to the authenticated methods, such as sending a transaction or even requesting the private key address, MetaMask will show a pop-up to the user asking for permission.
```
let signer = null;

let provider;
if (window.ethereum == null) {

    // If MetaMask is not installed, we use the default provider,
    // which is backed by a variety of third-party services (such
    // as INFURA). They do not have private keys installed,
    // so they only have read-only access
    console.log("MetaMask not installed; using read-only defaults")
    provider = ethers.getDefaultProvider()

} else {

    // Connect to the MetaMask EIP-1193 object. This is a standard
    // protocol that allows Ethers access to make all read-only
    // requests through MetaMask.
    provider = new ethers.BrowserProvider(window.ethereum)

    // It also provides an opportunity to request access to write
    // operations, which will be performed by the private key
    // that MetaMask manages for the user.
    signer = await provider.getSigner();
}
```












<br><br>
<br><br>
___________________________________________
___________________________________________
<br><br>
<br><br>

## Provider
- Ethers works closely with an ever-growing list of third-party providers to ensure getting started is quick and easy, by providing default keys to each service.

These built-in keys mean you can use ethers.getDefaultProvider() and start developing right away.

However, the API keys provided to ethers are also shared and are intentionally throttled to encourage developers to eventually get their own keys, which unlock many other features, such as faster responses, more capacity, analytics and other features like archival data.

When you are ready to sign up and start using for your own keys, please check out the Provider API Keys in the documentation.
- https://docs.ethers.org/v5/api-keys/

- A special thanks to these services for providing community resources:
  - Ankr
  - QuickNode
  - Etherscan
  - INFURA
  - Alchemy


<br><br>
<br><br>

### jsonrpc-provider
- https://docs.ethers.org/v5/api/providers/jsonrpc-provider/

<br><br>

### Custom RPC Backend
- Check here how to setup local
  - https://github.com/CyberT33N/lodestar-cheat-sheet/blob/main/README.md#quickstart-scripts

- If you are running your own Ethereum node (e.g. Geth) or using a custom third-party service (e.g. INFURA), you can use the JsonRpcProvider directly, which communicates using the link-jsonrpc protocol.

When using your own Ethereum node or a developer-base blockchain, such as Hardhat or Ganache, you can get access to the accounts with JsonRpcProvider-getSigner.
```javascript
// If no %%url%% is provided, it connects to the default
// http://localhost:8545, which most nodes use.
provider = new ethers.JsonRpcProvider(url)

// Get write access as an account by getting the signer
signer = await provider.getSigner()
```






















<br><br>
<br><br>
___________________________________________
___________________________________________
<br><br>
<br><br>

# Contracts
- https://docs.ethers.org/v6/getting-started/#starting-contracts
- A Contract is a meta-class, which means that its definition is derived at run-time, based on the ABI it is passed, which then determined what methods and properties are available on it.
- Since all operations that occur on the blockchain must be encoded as binary data, we need a concise way to define how to convert between common objects (like strings and numbers) and its binary representation, as well as encode the ways to call and interpret the Contract.

For any method, event or error you wish to use, you must include a Fragment to inform Ethers how it should encode the request and decode the result.

Any methods or events that are not needed can be safely excluded.

There are several common formats available to describe an ABI. The Solidity compiler usually dumps a JSON representation but when typing an ABI by hand it is often easier (and more readable) to use the human-readable ABI, which is just the Solidity signature.

<br><br>

## UniswapV3Factory
```javascript
import { ethers } from 'ethers';

import UniswapV3Factory from '@uniswap/v3-core/artifacts/contracts/UniswapV3Factory.sol/UniswapV3Factory.json' with { type: "json" }

const { abi } = UniswapV3Factory

// Default third party provider
const provider = ethers.getDefaultProvider()
// Custom provider
// const provider =  new ethers.JsonRpcProvider('http://localhost:8551')

// Erstellen eines Vertrags für die Uniswap V3 Factory
const factoryAddress = '0x1F98431Ab5cddB0572e6d72020f4a913956F1C04';
const factoryContract = new ethers.Contract(factoryAddress, abi, provider)
```
