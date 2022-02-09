# database of token transfers for public EOSIO chains

This service provides access to the full history of token transfers for several public EOSIO blockchains: EOS, Proton, Telos, and WAX. In EOS history, EIDOS and POW token are filtered out. The database contains irreversible transactions only.

The data is taken from EOSIO state history plugin, processed by [Chronicle](https://github.com/EOSChronicleProject), and processed by the [writer script](https://github.com/cc32d9/eosio_token_accounting). 

The [database schema](https://github.com/cc32d9/eosio_token_accounting/blob/master/eosio_token_accounting_tables.psql) consists of a set of tables for every chain, with blockchain name as a prefix: 

* `SYNC` contains the last processed block number for every chain.
* `%%_CURRENCIES` lists all known currencies, their decimal precision and multiplier.
* `%%_BALANCES` lists all current token balances for all accounts.
* `%%_TRANSFERS` contains token all transfers. It is indexed by `account_name`, which is either receiver or sender of a token, and `delta` defines if the token is transferred in or out. `other_party` is the sender or receiver account, respectively.
* `%%_FAILED_DECODING` contains transactions which could not be decoded due to ABI errors.


Public access to the data is available for any MySQL/MariaDB client. 

## access

* user: `eosio_token_accounting_ro`
* password: `eosio_token_accounting_ro`
* database: `eosio_token_accounting`

Host in Europe:

* host: `eu01.pub.bdata.one`
* port: `3303`


## examples

```
mysql --host=eu01.pub.bdata.one --port=3303 \
  --user=eosio_token_accounting_ro --password=eosio_token_accounting_ro --database=eosio_token_accounting

# all token transfers for the account during December 2021
select * from wax_TRANSFERS where account_name = 'cc32dnineexp' and 
  block_time >= '2021-12-01' and block_time < '2022-01-01';

```



---
Copyright (c) 2021 cc32d9@gmail.com
