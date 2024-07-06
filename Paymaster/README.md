
In this tutorial we will:

- Learn about paymasters.

- Review the testnet paymaster smart contract code.

- Use the testnet paymaster to pay transaction fees with our own ERC20 token.



## Prerequisites

1. Before you start, make sure that
[you’ve configured the %%zk_testnet_name%% in your browser wallet by following the instructions here](/build/connect-to-zksync).
2. In addition, fund your wallet with %%zk_testnet_name%% ETH using [one of the available faucets](/ecosystem/network-faucets).


# Deploy om Zksync 

# Learn How to How to deplpoy Dapp on zksync sepolia testnet


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


## Step1: Clone this repo

```
 git clone https://github.com/Vikash-8090-Yadav/ZKsyncTut.git

```

## Step2: Install libraries

```
yarn install 
```

## Step3: Compile the contract

```
yarn compile

```

![Screenshot from 2024-06-15 04-22-07](https://github.com/Vikash-8090-Yadav/ZKsyncTut/assets/85225156/0f767c05-75d8-485b-840f-221a72c5d50c)


## Step4: Replace your original contract in contract/youcntract.sol folder

## step5: Under deployment deploy/deploy.ts file
 - Replace with your contract artificat
 - Give the ocntructor Arguement (If any)
   

## Step6: Deploy the contract 

```
yarn deploy 
```

--network flag is optional


![Screenshot from 2024-06-15 04-22-58](https://github.com/Vikash-8090-Yadav/ZKsyncTut/assets/85225156/d2fe2f75-ea91-43eb-84d3-365cb9d48a66)

## Congrats! You have sucessfully deployed your First contract on zksync. which is not easy task. 




## Exceptional case

 - Learn how to handle compiling non-inlinable libraries. : https://docs.zksync.io/build/tooling/hardhat/compiling-libraries


#  Helpful Docs

Zksync Docs: https://docs.zksync.io/build
Zksync Community : https://code.zksync.io/ 




## What is a Paymaster?

Paymasters in the ZKsync ecosystem represent a groundbreaking approach to handling transaction fees.
They are special accounts designed to subsidize transaction costs for other accounts,
potentially making certain transactions free for end-users.
This feature is particularly useful for dApp developers looking
to improve their platform's accessibility and user experience by covering transaction fees on behalf of their users.

Every paymaster has the following two functions:

- `validateAndPayForPaymasterTransaction` : this function uses the transaction parameters (fields like `from`, `amount` , `to`
  ) to execute the required validations and pay for the transaction fee.

- `postTransaction`: this optional function runs after the transaction is executed.

![zksync paymaster](/images/101-paymasters/zksync-paymaster.png)

## Paymaster smart contract code

Although application developers are encouraged to create their own paymaster smart contract, ZKsync provides a testnet
paymaster for convenience and testing purposes.

::callout{icon="i-heroicons-exclamation-triangle" color="amber"}
The paymaster smart contract code is provided "as-is" without any express or implied warranties.
<br />

- Users are solely responsible for ensuring that their design, implementation,
  and use of the paymaster smart contract software complies with all applicable laws,
  including but not limited to money transmission, anti-money laundering (AML), and payment processing regulations.

- The developers and publishers of this software disclaim any liability for any legal issues that may arise from its use.
::

The testnet paymaster address is
[0x3cb2b87d10ac01736a65688f3e0fb1b070b3eea3](https://sepolia.explorer.zksync.io/address/0x3cb2b87d10ac01736a65688f3e0fb1b070b3eea3)


        ```
        // SPDX-License-Identifier: MIT

        pragma solidity 0.8.20;

        import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

        import "./interfaces/IPaymaster.sol";
        import "./interfaces/IPaymasterFlow.sol";
        import "./L2ContractHelper.sol";

        // This is a dummy paymaster. It expects the paymasterInput to contain its "signature" as well as the needed exchange rate.
        // It supports only approval-based paymaster flow.
        contract TestnetPaymaster is IPaymaster {
            function validateAndPayForPaymasterTransaction(
                bytes32,
                bytes32,
                Transaction calldata _transaction
            ) external payable returns (bytes4 magic, bytes memory context) {
                // By default we consider the transaction as accepted.
                magic = PAYMASTER_VALIDATION_SUCCESS_MAGIC;

                require(msg.sender == BOOTLOADER_ADDRESS, "Only bootloader can call this contract");
                require(_transaction.paymasterInput.length >= 4, "The standard paymaster input must be at least 4 bytes long");

                bytes4 paymasterInputSelector = bytes4(_transaction.paymasterInput[0:4]);
                if (paymasterInputSelector == IPaymasterFlow.approvalBased.selector) {
                    // While the actual data consists of address, uint256 and bytes data,
                    // the data is not needed for the testnet paymaster
                    (address token, uint256 amount, ) = abi.decode(_transaction.paymasterInput[4:], (address, uint256, bytes));

                    // Firstly, we verify that the user has provided enough allowance
                    address userAddress = address(uint160(_transaction.from));
                    address thisAddress = address(this);

                    uint256 providedAllowance = IERC20(token).allowance(userAddress, thisAddress);
                    require(providedAllowance >= amount, "The user did not provide enough allowance");

                    // The testnet paymaster exchanges X wei of the token to the X wei of ETH.
                    uint256 requiredETH = _transaction.gasLimit * _transaction.maxFeePerGas;
                    if (amount < requiredETH) {
                        // Important note: while this clause definitely means that the user
                        // has underpaid the paymaster and the transaction should not accepted,
                        // we do not want the transaction to revert, because for fee estimation
                        // we allow users to provide smaller amount of funds then necessary to preserve
                        // the property that if using X gas the transaction success, then it will succeed with X+1 gas.
                        magic = bytes4(0);
                    }

                    // Pulling all the tokens from the user
                    try IERC20(token).transferFrom(userAddress, thisAddress, amount) {} catch (bytes memory revertReason) {
                        // If the revert reason is empty or represented by just a function selector,
                        // we replace the error with a more user-friendly message
                        if (revertReason.length <= 4) {
                            revert("Failed to transferFrom from users' account");
                        } else {
                            assembly {
                                revert(add(0x20, revertReason), mload(revertReason))
                            }
                        }
                    }

                    // The bootloader never returns any data, so it can safely be ignored here.
                    (bool success, ) = payable(BOOTLOADER_ADDRESS).call{value: requiredETH}("");
                    require(success, "Failed to transfer funds to the bootloader");
                } else {
                    revert("Unsupported paymaster flow");
                }
            }

            function postTransaction(
                bytes calldata _context,
                Transaction calldata _transaction,
                bytes32,
                bytes32,
                ExecutionResult _txResult,
                uint256 _maxRefundedGas
            ) external payable override {
                // Nothing to do
            }

            receive() external payable {}
        }
    ```
  ::
::

In the `validateAndPayForPaymasterTransaction` it is:

1. Checking that the paymasterInput is `approvalBased`.
2. Checking that the allowance of a given ERC20 is enough.
3. Transferring the transaction fee (`requiredETH`) in ERC20 from the user’s balance to the paymaster.
4. Transferring the transaction fee in ETH from the paymaster contract to the bootloader.

## How to send a transaction through a paymaster?

In order to send a transaction through a paymaster, the transaction must include the following additional parameters:

- `paymasterAddress`: the smart contract address of the paymaster
- `type`: should be `General` or `ApprovalBased` (to pay fees with ERC20 tokens)
- `minimalAllowance`: the amount of ERC20 tokens to be approved for spending (for `approvalBased` type paymasters only).
- `innerInput`: any payload we want to send to the paymaster (optional).

We’ll see an example next.

## Interacting with the testnet paymaster

We’re going to interact with the `ZeekMessages.sol` contract that we created in the first tutorial and use the
ERC20 token that we deployed in the second tutorial to pay the transaction fees.

::content-switcher
---
items: [{
  label: 'Atlas',
  partial: '_paymaster_intro/_atlas_paymaster_intro'
}, {
  label: 'Remix',
  partial: '_paymaster_intro/_remix_paymaster_intro'
}]
---
::

## Takeaways

- Paymasters on ZKsync allow any account to pay fees with ERC20 tokens or enable gasless transactions.

- Paymasters are smart contracts that can have any validations and rules.
- To send a transaction through a paymaster, we only need to include additional parameters in the transaction.

## Next steps


