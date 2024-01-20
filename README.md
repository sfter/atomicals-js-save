# Atomicals Javascript Library Special Version

[中文文档](https://github.com/atlaslabsapp/atomicals-js/blob/handle_no_reveal/README_cn.md)


This version is only for recovery your BTC when a commit tx is broadcast but no reveal tx is broadcast.
It looks like your BTC is sent to a address that looks unfamiliar to you, and you got nothing return.
- When set `refund=true` in `.env` config file, it try to refund from commit tx. 
  You should set a much lower `newSatsbyte` or `--satsbyte` than the original one, or you may get only a few sats back.
- When set `refund=true`, it try to construct the reveal tx, which continue to mint. 
  It is highly recommend when the item is not minted out

**DO NOT USE IT FOR ANY OTHER PURPOSE!**

**THE AUTHOR IS NOT RESPONSIBLE FOR ANY LOSSES!**

**USE AT YOUR OWN RISK!**

## Usage

1. Clone the modified code, switch to branch `handle_no_reveal` and compile it
    ```
    git clone https://github.com/atlaslabsapp/atomicals-js.git
    cd atomicals-js
    git checkout -b handle_no_reveal origin/handle_no_reveal
    yarn install
    yarn run build
    ```

2. Config `.env` file with your mint info. Try you best to fill all params under `important`. 

   Based on the status of different parameter inputs, the expected completion time varies:
    - If both time and nonce are filled in correctly: immediate (or pending reveal bitwork completion)
    - If only time is filled in correctly: a few hours to several days
    - If only nonce is filled in correctly: a few seconds to several minutes
    - If neither time nor nonce is filled in: virtually impossible， find 'time' in your commit tx log or https://www.blockchain.com/explorer/

    ``` yaml
    #################
    # Recovery configs
    #################
    
    # important
    
    # refund=true: create a refund tx, recommend when item already minted out
    # refund=false: create a reveal tx, recommend when item not minted out
    refund=false
    # broadcast=true: broadcast your tx automatically like the original one
    # broadcast=false: only print the rawTx hex, you can check and broadcast it manually
    broadcast=false
    # commitTxid is the tx which sends your BTC to a address that looks unfamiliar to you, it is required
    commitTxid=
    # time and nonce is required to use the BTC from the unfamiliar address which commit tx sent to
    # You can get from console log like 'got one finalCopyData:{"args":{"bitworkc":"000000","bitworkr":"6238",
    #   "mint ticker":"sophon","nonce":7101230,"time":1705506662}}'
    # If you are using a new version which do not have such log, config nonce=0 and leave time blank
    time=
    nonce=
    
    # optional
    
    # When refund=true, BTC will be sent to this address
    # If left blank, it defaults to the funding address of your wallet
    refundAddress=
    # Set new feerate for the tx created. It overrides --satsbyte
    # If left blank, it defaults to the --satsbyte param in your command
    # You can also just left blank and change it with the --satsbyte param
    newSatsbyte=
    # Pubkey used to construct reveal p2tr address.
    # It is only for an advanced user to run without the correspond wallet json file
    # If left blank, it will calculate from the funding address of your wallet
    childNodeXOnlyPubkey=
    ```

3. Copy your wallet file to `./wallets/wallet.json`, or config your wallet file path in `.env`

4. Rerun your mint command. It is recommended to add `--rbf` params so that you can rerun the program to replace your tx in some case

5. If all things go well, there will be a rawtx hex shown in console log.
   - Please use tools like [sparrow](https://sparrowwallet.com/) or https://live.blockcypher.com/btc/decodetx/ to verify that the tx hex is what your need.
   - Then broadcast it manually at https://mempool.space/tx/push .
   - You can also set `broadcast=true` in `.env` so the program can broadcast your tx automatically like the original one, but it is not recommend since the code is not fully tested.


## Donate
If this special version made a difference for you, a donation would be greatly appreciated.

- BTC: `bc1pzgt4jhrmky5ve7xvh6vddvft82qwatmxe06r9f9y63yajwgfgg4q23jadt`
- USDT (TRC20): `TXmQ9Z9u776umAeCH5W5rUgMk9rfaL64Fx`

It's the support of users like you that enables me to continue my work.
