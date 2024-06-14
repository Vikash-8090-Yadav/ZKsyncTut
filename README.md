# Learn How to create  a  Login page (Connect with metamask) & How to deplpoy Dapp on zksync sepolia testnet


# zkSync Overview

zkSync is a Layer 2 scaling solution for Ethereum, utilizing zero-knowledge rollups (zk-Rollups) to enhance the network's scalability and reduce transaction costs while maintaining security and decentralization.

## Key Features of zkSync

1. **Scalability:**
   - zkSync significantly increases Ethereum's transaction throughput, processing thousands of transactions per second (TPS) compared to Ethereum's base layer.

2. **Low Transaction Costs:**
   - Transactions on zkSync are much cheaper than those on the Ethereum mainnet due to the bundling of multiple transactions into a single batch, reducing the overall cost.

3. **Security:**
   - zkSync inherits Ethereum's security by using zk-Rollups. In this mechanism, a smart contract on the Ethereum mainnet maintains the state of all transactions processed off-chain, ensuring they are valid through cryptographic proofs.

4. **Instant Withdrawals:**
   - Users can withdraw funds back to the Ethereum mainnet instantly, unlike other Layer 2 solutions that might have longer withdrawal times.

5. **EVM Compatibility:**
   - zkSync aims to support EVM (Ethereum Virtual Machine) compatibility, allowing developers to easily port their existing Ethereum-based applications to zkSync with minimal modifications.

6. **User Experience:**
   - zkSync provides a seamless user experience with faster transaction confirmations and lower fees, which is crucial for the adoption of decentralized applications (dApps).

## Conclusion

zkSync represents a promising solution to Ethereum's scalability issues, leveraging advanced cryptographic techniques to offer a scalable, secure, and cost-effective platform for a wide range of applications. Its development and integration into the broader Ethereum ecosystem highlight its potential to significantly enhance the user and developer experience on Ethereum.

## Testnet Faucet 
please follow this guide https://docs.zksync.io/build/tooling/network-faucets.html


#  How to Create Login page  to connect with metamask directly with zksync

Prerequiste:
- Knowledge of solidity fundamentals & Javascript
- Basics of Blockchain
- Familiar  with L2 rollups


## Step 1 : Craete a network  object 

```

const networks = {
  zkSyncSepoliaTestnet: {
    chainId: `0x${Number(300).toString(16)}`,
    chainName: "zkSyncSepoliaTestnet",
    nativeCurrency: {
      name: "zkSyncSepoliaTestnet",
      symbol: "ETH",
      decimals: 18,
    },
    rpcUrls: ["https://sepolia.era.zksync.dev"],
  },
};

```

## Step2 : Create a habdle Login function 

```
  const handleLogin = async () => {
    setLoading(true);
    
    if(typeof window.ethereum =="undefined"){
      console.log("PLease install the metamask");
  }
  let web3 =  new Web3(window.ethereum);
 
  if(web3.network !=="zkSyncSepoliaTestnet"){
      await window.ethereum.request({
          method:"wallet_addEthereumChain",
          params:[{
              ...networks["zkSyncSepoliaTestnet"]
          }]
      })
  }

```

After that you can call this hanndle login fucntion  at any Login button and then  handle Login  check if the required network is selected if not then it request from wallet to show signature for zksync testnet

Below is the image if the network is diffrent 


![Screenshot from 2024-06-15 03-39-37](https://github.com/Vikash-8090-Yadav/ZKsyncTut/assets/85225156/af41e487-7085-4b66-9270-7409511e6b8e)


Here's the full code with the Login component 



Here's the quick demo 



https://github.com/Vikash-8090-Yadav/ZKsyncTut/assets/85225156/3e3703a8-0398-42e0-9f04-5c9a96cd3349




You can find the same code here as well: https://github.com/Vikash-8090-Yadav/FundsDao/blob/main/Frontend/src/components/nav.jsx

## How to deploy  any contract on zksyncsepolia  testnet 

For every deployment there is some sort of steps which every user  have to follow some basics steps 

- Smart contract should be familiar with zksync-cli & solc
- Install harhdat plugins for zksync
- create the deployment script


## Step1:  Create the zksync hardhat template project with zksync cli 


