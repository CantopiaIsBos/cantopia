# Cantopia

Cantopia is the homepage for Canto. 

Users can find and use all apps for Canto in a single place: core apps, DeFi, NFT mints, etc

Devs can view the source code of apps (frontends for Canto protocols), fork them, and deploy their own version. All of the app code (HTML/CSS/JS)  is stored on the NEAR blockchain, creating a trust-minimized experience for users and devs. 

If we win the hackathon (and even if we don't) we will use all of the funds to launch a bounty program to get high quality, trustless frontends built for the top Canto protocols!


## How it Works

Cantopia is made up of three parts:

### Gateway (cantopia.gg)
The gateway is a web app with a specially designed virtual machine that loads and runs frontends for Canto protocols. The code for these frontends is stored on the NEAR blockchain.

### Apps
In Cantopia, "apps" are frontends for Canto protocols that are stored entirely on-chain. The code for these apps can be viewed in a gateway, similar to viewing a smart contract in Etherscan. Developers can fork these apps and deploy their own versions, or even compose apps together ([see this as an example]())

### Blockchains
All apps on Cantopia point to the Canto blockchain (naturally). Cantopia apps are interfaces for smart contracts on Canto. The source code for the apps (frontends) is on NEAR, due to it's ability to very cheaply store HTML/CSS/JS (a few cents).

## Setup & Development

Initialize repo:
```
yarn
```

Start development version:
```
yarn start
```

## App (frontend) example

Profile view 

```jsx
const sender = Ethers.send("eth_requestAccounts", [])[0];

if (!sender) return "Please connect your wallet";

const erc721Abi = fetch(
  "https://gist.githubusercontent.com/kcole16/ef017dec9da7c1acb387de643835e840/raw/c4e0bf37092b4fd2832e971e5210bd70ca2345ab/erc721.abi.json"
);
if (!erc721Abi.ok) {
  return "Loading...";
}

const iface = new ethers.utils.Interface(erc721Abi.body);

initState({
  contractAddress: "0x25EEe23b20C61E03596B7FAd3e87E20E7AF6f55C",
  amount: "1",
});

const mintTokens = () => {
  const erc721 = new ethers.Contract(
    state.contractAddress,
    erc721Abi.body,
    Ethers.provider().getSigner()
  );

  let cost = 69;
  let subtotal = (state.amount * cost).toString();
  let value = ethers.utils.parseUnits(subtotal, 18);

  erc721.mint(state.amount.toString(), {
    from: sender,
    value: value,
  });

  console.log("transactionHash is " + transactionHash);
};

return (
  <>
    <h3>Mint BonkToes</h3>
    <div class="mb-3">
      <label for="amount" class="form-label">
        Amount
      </label>
      <input
        value={state.amount}
        class="form-control"
        id="amount"
        placeholder=""
        onChange={(e) => State.update({ amount: e.target.value })}
      />
    </div>
    <div class="mb-3">
      <button onClick={mintTokens}>Mint</button>
    </div>
  </>
);
```


