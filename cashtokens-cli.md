# Kumaly CLI

This is an overview of the kumaly CLI that work in node and in the browser.

## Basic stuff

### Help

```
$> kumaly help

Usage: kumaly [OPTIONS] COMMAND

Kumaly command line tool

Account Commands:
  account           Manage your account
  whoami            Show your current account and wallet

General Commands:
  address           Show addresses of accounts and contracts

Chains Commands:
  bch               Interact with Bitcoin Cash chain


Run 'kumaly COMMAND --help' for more information on a command.
```

### Whoami

```
$> kumaly whoami
@me+w
```

@account allow to access a specific account in the wallet, by default the first account is @me

+w allow to access a specific wallet, the default wallet is +w

Later on you will see URI, that referers to that format, your URI is @me+w

### Address

What is my BCH address?

```
$> kumaly address @me#bch+w
bchreg:zqmsw42r6p4wn9tj8swdfmc5y4zkn7uzay5yft06k9
```

What is my EVM address?

```
$> kumaly address @me#eosevm+w

0xfad6d1265ce3c4939b7ea7502564eef6a40e0361
```

## BCH

Kumaly BCH library is built on top of the awesome mainnet.cash library.

```
$> kumaly bch help

Usage: kumaly bch COMMAND

Interact with Bitcoin Cash chain

Chain Commands:
  height      Show the last block height
  transaction Show the decoded transaction datas

Address Commands:
  balance     Show the current address balance
  history     Show the address history of transactionsIds
  transfer    Transfer coins, fts and nfts
  utxos       Show the current address utxos

Fungible Tokens Commands:
  ftbalance   Show the address FTs balances
  ftcreate    Create a new FT

Non Fungible Tokens Commands:
  nftbalance  Show the address NFTs balances
  nftcreate   Create a new NFT
  nftlist     List the address NFTs
  nftmint     Mint a new NFT
```

### Height

```
$> kumaly bch height

215
```

### Balance

```
$> kumaly bch balance help

Usage: kumaly bch balance ADDRESS|URI [OPTIONS]

Show the current address balance

Options:
      --unit=UNIT      Sets the unit. Valid options are `sats` and `bch` (default).
      --sats           Sets the unit to satoshis.
      --bch            Sets the unit to bch.
```

Let's check my BCH balance:

```
$> kumaly bch balance @me+w

10.00

$> kumaly bch balance @me+w --sats

1000000000
```

Let's check an other address:

```
$> kumaly bch balance bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4

0.00
```

### UTXOs

```
$> kumaly bch utxos @me+w

{ "txid": "492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985", "vout": 0, "satoshis": 1000000000, "height": 0 }
```

### Fungible Token

```
$> kumaly bch ftcreate help

Usage: kumaly bch ftcreate URI amount [OPTIONS]

Create a new FT

Options:
      --unit=UNIT            Sets the unit. Valid options are `sats` and `bch` (default).
      --sats                 Sets the unit to satoshis.
      --bch                  Sets the unit to bch.
      --address=ADDRESS      Address receiving the token, by default current account.
```

Let's create a new FT:

```
$> kumaly bch ftcreate @me+w 1000

transactionId: 77f7c787af588ec5f2e9ac13a312f3179e2e3d21e18730053f08e953851db44f
tokenId: 492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985
```

```
$> kumaly bch ftbalance @me+w

492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985 1000.00
```

### Non Fungible Token

```
$> kumaly bch nftcreate help

Usage: kumaly bch nftcreate ADDRESS|URI [OPTIONS]

Create a new NFT

Options:
      --address=ADDRESS            Address receiving the token, by default current account.
      --capability=CAPABILITY      Sets the token capability. Valid options are `none` (default), `minting`, and `mutable`.
      --commitment=COMMITMENT      Sets the token commitment.
```

Let's create a new NFT token with minting capability (baton):

```
$> kumaly bch nftcreate @me+w --capability=minting

transactionId: 156da93143895d2ac0e31f7bc4748d9cfeb8da786517adf556861f239efa183a
tokenId: 20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0
```

```
$> kumaly bch nftbalance @me+w

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 1
```

Can we get more details ?

```
$> kumaly bch nftlist @me+w

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 minting
```

Let's mint 3 new tokens with that baton:

```
$> kumaly bch nftmint help

Usage: kumaly bch nftmint ADDRESS|URI TOKEN [OPTIONS]

Mint a new NFT

Options:
      --to-nft=TO_NFT      Specify a NFT mint: `prefix:address,token:TOKEN[,commitment:COMMITMENT][,capability:CAPABILITY][,value:VALUE]`

```

```
$> kumaly bch nftmint @me+w 20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 \
--to=bchreg:zqmsw42r6p4wn9tj8swdfmc5y4zkn7uzay5yft06k9,commitment:1 \
--to=bchreg:zqmsw42r6p4wn9tj8swdfmc5y4zkn7uzay5yft06k9,commitment:2 \
--to=bchreg:zqmsw42r6p4wn9tj8swdfmc5y4zkn7uzay5yft06k9,commitment:3

transactionId: 37ab78889d000e6f16353c65f0d74c5ae7ce50e6b011c204f200315b2b79c5dd
tokenId: 20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0
```

```
$> kumaly bch nftbalance @me+w

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 4
```

```
$> kumaly bch nftlist @me+w

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0    minting
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 01 none
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 02 none
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 03 none
```

### Transfer

```
$> kumaly bch transfer help

Usage: kumaly bch transfer URI [OPTIONS]

Transfer coins, fts and nfts

Options:
      --unit=UNIT          Sets the unit. Valid options are `sats` and `bch` (default).
      --sats               Sets the unit to satoshis.
      --bch                Sets the unit to bch.
      --to-bch=TO_BCH      Specify a BCH transfer: `prefix:address,value:VALUE`
      --to-ft=TO_FT        Specify a FT transfer: `prefix:address,token:TOKEN,amount:AMOUNT[,value:VALUE]`
      --to-nft=TO_NFT      Specify a NFT transfer: `prefix:address,token:TOKEN[,commitment:COMMITMENT][,capability:CAPABILITY][,value:VALUE]`

```

Let's transfer 3 BCH, 100 of our FT and the NFT #2:

```
$> kumaly bch transfer @me+w \
--to-bch=bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4,value:3 \
--to-ft=bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4,token:492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985,amount:100 \
--to-nft=bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4,token:20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0,commitment:02

transactionId: e94e3a26cc12fb45fd674832b89b9f456e07c4dd26d681823f0417c2e4d6fcc5
```

Did everything work well?

```
$> kumaly bch balance @me+w

7.00

$> kumaly bch balance bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4

3.00
```

```
$> kumaly bch ftbalance @me+w

492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985 900.00

$> kumaly bch ftbalance bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4

492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985 100.00
```

```
$> kumaly bch nftlist @me+w

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 minting
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 01 none
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 03 none

$> kumaly bch nftlist bchreg:zqfd3dkczvqhdcxay477wfv89rkj5rfqwukj6xejj4

20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0 02 none
```

### History

```
$> kumaly bch history @me+w

492954afb50bdbbb03420b4547a9c4dafde5d77672ca1221759d8b706ecd7985
156da93143895d2ac0e31f7bc4748d9cfeb8da786517adf556861f239efa183a
20032cef97637d0923399b3edaf1ef7d8bb70bd8322683a489d6fcffb10fb3b0
37ab78889d000e6f16353c65f0d74c5ae7ce50e6b011c204f200315b2b79c5dd
447b3479a37e9e6753434d7f0660294d9b6ee5b16e510265a36f5a2aba9f6fd4
7697a80278a6eef79925bf2f6ba7331d8c8606681561253c16b87202239d7e47
77f7c787af588ec5f2e9ac13a312f3179e2e3d21e18730053f08e953851db44f
e94e3a26cc12fb45fd674832b89b9f456e07c4dd26d681823f0417c2e4d6fcc5
```

### Transaction

```
$> kumaly bch transaction e94e3a26cc12fb45fd674832b89b9f456e07c4dd26d681823f0417c2e4d6fcc5

{ "hash": "e94e3a26cc12fb45fd674832b89b9f456e07c4dd26d681823f0417c2e4d6fcc5", "hex": ... }
```
