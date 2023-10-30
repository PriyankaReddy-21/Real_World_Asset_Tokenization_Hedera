[![GitHub license](https://img.shields.io/github/license/hashgraph/hedera-hts-demo)](https://github.com/hashgraph/hedera-hts-demo/blob/master/LICENSE)
[![Discord](https://img.shields.io/badge/discord-join%20chat-blue.svg)](https://hedera.com/discord)

# Hedera Token Service (HTS) demo

This demo is a user interface written in Javascript (Vue.JS) to illustrate the use of the Hedera Token Service. When first launched, the demo will create four accounts as follows:
* An account for the owner/admin of new tokens - Issuer
* Two accounts representing users (wallet holders) that will use the token, Alice and Bob
* Another account representing a marketplace (escrow) for the purpose of holding tokens that have been offered for sale

Each account is credited some hBar to fund their activity on Hedera.
The accounts of Alice, Bob and marketplace are pre-associated with all tokens during token creation.
This is done to enable seamless tranfers of tokens among all parties, for demo purpose.
For a more practical implementation, an extra button / page for token association should be created that enables the same for any other user account.

Here, the Issuer can create both fungible or non-fungible tokens based on their requirements.

1. Fungible token :
   - Fungible tokens can be created and hosted for tokenising assets like a company or a property like house etc.
   - The tokens created so can be used for the purposes of ICO (Initial coin offering), STO (Stock / Loyalty), bond etc.
   - The actual purpose of the token can be determined based on the parameters we select during token creation.
   - Buyers can buy these tokens from the marketplace or directly from the issuer by paying the equivalent Hbar amount.

2. Non-fungible token :
   - NFT can be created for tokenising unique items like art, painting, some copyrighted content, a movie, a piece of music etc.
   - NFT will be created as a single token only and minting / burning of this is not possible.
   - It can only be sold to other users for equivalent Hbar amount.

The demo enables you to:
* Create tokens
* Mint and Burn
* Associate and Dissociate tokens to/from accounts
* Manage KYC and Freeze for token/account relationships
* Transfer from treasury (owner) to users
* Transfer between users
* Atomically transfer up to two tokens between users, with an optional hBar payment as one atomic transaction
* Transfer tokens to a marketplace escrow account along with an offer price in hBar

For convenience, accounts and tokens created during the demo will be persisted to a cookie and will be available when the demo is restarted.

_Note: This is purely for demonstration purposes and should not be used as-is in production_

## Prerequisites

* A testnet or mainnet account (head to https://portal.hedera.com to create an account if you don't have one)
* Node.js v14.9.0
* Yarn 1.22.10
* Docker 
* Docker compose (optional)

## Environment files

The project requires an environment file to be setup. 
First, copy `.env.sample` to `.env`
Edit `.env` and setup the following variables

* VUE_APP_OPERATOR_ID=0.0.xxxx Input your operator id 
* VUE_APP_OPERATOR_KEY=302xxx Input your private key
* VUE_APP_INITIAL_BALANCE=100
* VUE_APP_NETWORK=testnet (or mainnet)

optionally you may set
* VUE_APP_MAX_QUERY_PAYMENT=10 max allowed query payment
* VUE_APP_MAX_TX_FEE=10 max allowed transaction fee

## I just want to run it quickly

You can deploy the UI with docker-compose after you have edited the `.env` file above.

Build
```shell script
docker-compose build --no-cache
```

Run
```shell script
docker-compose up
```

Note: `docker-compose` build is only necessary the first time, or if you make changes to the code or `.env` to (re)build the images. 

## I want to build it myself

### Project setup
```
yarn install
```

#### Compiles and hot-reloads for development
```
yarn serve
```

#### Compiles and minifies for production
```
yarn build
```

#### Lints and fixes files
```
yarn lint
```

## Using the UI

Navigate to the URL output by the `serve` command (e.g. http://localhost:8080/) to access the UI.
If you are running docker-compose, the url is `http://localhost:8080`.

The Header has links for the following:
* An admin page where you can manage tokens
* Accounts for the users
* A `+` button to add a token
* A button to show a list of transactions that were executed while running the demo (newest at the top)
* A button to reset (nuke) the demo. This will remove all traces of the tokens and accounts in your browser and re-create a clean demo environment with three new accounts.

Once a token is created, you can:
* Show accounts associated with the token
* Mint (If a supply key was provided when the token was created)
* Burn (If a supply key was provided when the token was created) 
* Transfer from treasury to users
Note: Update and Delete are not currently supported

Clicking on the accounts button (left most) will show which accounts are currently associated with the token, from there you can:
* Freeze/UnFreeze an account in relation to the token (if a freeze key was provided when the token was created)
* Grant or Revoke KYC for an account in relation to the token (if a KYC key was provided when the token was created)
* Wipe an amount of tokens from an account (if a wipe key was provided when the token was created)

Choosing one of the user accounts in the header allows you to associate or dissociate the account from a token.
Once associated (and subject to the account being KYCd and unfrozen if appropriate for the token), you can transfer to the other user account.

## NFTs

Support for creating NFTs is demonstrated in the composer whereby a set of templates (driven from `/public/tokenTemplates.json`) are available within the UI when creating an NFT.
Each template carries a set of properties that are input during the token creation.
These properties (along with an image if necessary) as then stored in an immutable file on Hedera, the resulting FileId is used to define the symbol for the token, e.g. HEDERA://0.0.xxxx.

You may edit or add to the templates by editing the `/public/tokenTemplates.json` file, following guidelines from [the vjsf component](https://koumoul-dev.github.io/vuetify-jsonschema-form/latest/about).
Note that if you wish to include a picture in your NFT specification, the property must be called `photo` since the UI depends on that field value.

_Note: An alternative to using files on Hedera would be to host the file on a shared location and use a hash of the file as the symbol for the token so that the validity of the file can be verified at any time.
We demonstrate usage of IPFS storage using [nft.storage](https://nft.storage) integration. You can create your API key on https://nft.storage and add it to your .env file to enable IPFS upload._

## Marketplace

A pseudo-market place is enabled in the demo, this enables a token to be transferred to a market place (an escrow account of sorts) along with an offer price in hBar.

One a token has been transferred to the market place, Alice or Bob can request transfer of the token from the marketplace to their account in exchange for the offered hBar value whereby they will own the token in exchange for the hBar value which will be transferred to the account that transferred the token to the market place in the first place.

_Note: If Alice transferred a token to the market place, she's not able to buy the token, only Bob can. If the issuer (owner) of the token transferred to the marketplace, both Bob and Alice can buy it._

## Contributing

Contributions are welcome. Please see the [contributing](CONTRIBUTING.md) guide to see how you can get
involved.

## Code of Conduct

This project is governed by the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are
expected to uphold this code of conduct. Please report unacceptable behavior to [oss@hedera.com](mailto:oss@hedera.com)

## License

[Apache License 2.0](LICENSE)
