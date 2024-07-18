# daily activity database for public EOSIO chains

This service provides daily activity counters for every contract, action and user, for several public EOSIO blockchains: EOS, FIO, XPR (ex-Proton), Telos, Libre, and WAX.

The data is taken from EOSIO state history plugin, processed by [Chronicle](https://github.com/EOSChronicleProject), and processed by the [writer script](https://github.com/cc32d9/eosio_activity_db). 

The [database schema](https://github.com/cc32d9/eosio_activity_db/blob/master/eosio_activity_tables.psql) consists of a set of tables for every chain, with blockchain name as a prefix: 

* `SYNC` contains the last processed block number for every chain.
* `%%_CONTRACTS` lists all known contracts
* `%%_NEWACCOUNT` stores the account creation history
* `%%_DAILY_COUNTS` has daily counters for every contract, action and authorizer
* `%%_DAILY_ACTIONS` has daily counters for every contract and action
* `%%_DAILY_PAYIN` has a daily summary of all token transfers toward the contracts
* `%%_DAILY_PAYOUT` has a daily summary of all token transfers from the contracts
* `%%_AGGR_COUNTS` is an aggregation of `%%_DAILY_COUNTS`, providing a number of unique authorizers
* `%%_AGGR_PAYIN` and `%%_AGGR_PAYOUT` aggregate the data from `%%_DAILY_PAYIN` and `%%_DAILY_PAYOUT`, correspondingly

Public access to the data is available for any MySQL/MariaDB client. 

## access

* user: `eosio_activity_ro`
* password: `eosio_activity_ro`
* database: `eosio_activity`

Host in Europe:

* host: `eu01.pub.bdata.one`
* port: `3301`


## examples

```
mysql --host=eu01.pub.bdata.one --port=3301 \
  --user=eosio_activity_ro --password=eosio_activity_ro --database=eosio_activity


# top 30 contracts by number of executed actions on a specific day

select count(distinct authorizer) as USERS, sum(counter) as CNT, contract 
from eos_DAILY_COUNTS where xday = '2021-11-09' group by contract  order by CNT desc limit 30;

# top 30 accounts that authorized actions on a specific contract during the day

select authorizer, action, counter from eos_DAILY_COUNTS 
where xday = '2021-11-09' and contract='gravyhftdefi' order by counter desc limit 20;

# daily unique users for a month

select count(distinct authorizer) as USERS, xday from eos_DAILY_COUNTS 
where xday >= '2021-10-01' and xday < '2021-11-01' group by xday;

```



---
Copyright (c) 2021 cc32d9@gmail.com
