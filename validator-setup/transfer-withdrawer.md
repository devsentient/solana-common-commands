# Transfer Withdrawer to another entity

Please run the following commands from the machine that contains your new withdraw key, and do not upload your new withdraw key to your solana validator.

### 1. Change authorized withdrawer
- create a nonce account if you don’t have one:
```
solana-keygen new -o nonce-keypair.json
```
You will get output that looks like this:
```
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): 

Wrote new keypair to nonce-keypair.json
==================================================================================
pubkey: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
==================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
plastic flame beauty together small hidden run planet faculty august weird arrange
==================================================================================
```
Copy the value that you see for pubkey. It will be referred to as PUBKEY henceforth.
Next, let's generate a nonce account with the newly created key pair. 

You should specify another key to be your <NONCE-AUTH-KEYPAIR>, and keep it consistent throughout:
```
solana create-nonce-account nonce-keypair.json 0.1 -k <NONCE-AUTH-KEYPAIR> -u m
```
- print out nonce info:
```
  solana nonce-account nonce-keypair.json -u m
```
You will get output that looks like this:
```
Balance: 0.1 SOL
Minimum Balance Required: 0.00144768 SOL
Nonce blockhash: BBBBBBBBBBBBBBBBBBBBBBBBBBBBBB
Fee: 5000 lamports per signature
Authority: CCCCCCCCCCCCCCCCCCCCCCCCCCCC
```
Note the nonce blockhash value. It will be referred to as BLOCKHASH. 
  
Note the authority. It will be referred to as NONCE-AUTH-KEYPAIR.
  
FEE_PAYER_KEYPAIR can be any account that has some SOL. It can be the same as your nonce authority keypair. 
- use nonce info from above:
```
solana vote-authorize-withdrawer-checked --sign-only -u m --blockhash <BLOCKHASH> --fee-payer <FEE_PAYER_KEYPAIR> --nonce <PUBKEY> --nonce-authority <NONCE-AUTH-KEYPAIR>  DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 DNodec2t2PZjhtGkBwhXUpg8gpfPph5QSawh8C91RX8b <your-new-authorized-withdraw-keypair>

```
- send the current owner (Shakudo) the printout from the above command
- send Shakudo your nonce pubkey, nonce auth pubkey, and your fee-payer pubkey
- confirm the pubkey of your new authorized withdrawer


[no action needed] Shakudo will run a command on our side like the following
```
solana vote-authorize-withdrawer-checked <VOTE_ACCOUNT_PUBKEY> <CURRENT_WITHDRAWER_KEYPAIR> <NEW_WITHDRAWER_PUBKEY> --blockhash <blockhash-from-client> --nonce <nonce-pubkey-from-client> --nonce-authority <nonce-auth-pubkey-from-client> --signer <signer-from-client> -k <CURRENT_WITHDRAWER_KEYPAIR> -u m --fee-payer <fee-payer-from-client>

## e.g.
solana vote-authorize-withdrawer-checked DeNodee9LR1WPokmRqidmAQEq8UbBqNCv6QfFTvU6k69 withdrawer.json <NEW_WITHDRAWER_PUBKEY> --blockhash <blockhash-from-client> --nonce <nonce-pubkey-from-client> --nonce-authority <nonce-auth-pubkey-from-client> --signer <from-client> -k withdrawer.json -u m --fee-payer <fee-payer-from-client>
```

### 2. Create staking account or set existing staking account with your withdraw keypair
To create a new staking account:
```
solana create-stake-account --from <KEYPAIR> stake-account.json <AMOUNT> \
    --stake-authority <KEYPAIR> --withdraw-authority <KEYPAIR> \
    --fee-payer <KEYPAIR>
```
To set an existing staking account:
```
solana stake-authorize <STAKE_ACCOUNT_ADDRESS> \
    --stake-authority <KEYPAIR> --new-stake-authority <PUBKEY> \
    --fee-payer <KEYPAIR>
```
