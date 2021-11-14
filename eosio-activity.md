# daily activity database for public EOSIO chains

This service provides daily activity counters for every contract, action and user, for several public EOSIO blockchains: EOS, FIO, Proton, Telos, UX, and WAX.

The data is taken from EOSIO state history plugin, processed by [Chronicle](https://github.com/EOSChronicleProject), and processed by the [writer script](https://github.com/cc32d9/eosio_activity_db). 

Public access to the data is available for any MySQL/MariaDB client. 

### Host: eu01.pub.bdata.one

* host: `eu01.pub.bdata.one`
* port: `3301`
* user: `eosio_activity_ro`
* password: `eosio_activity_ro`
* database: `eosio_activity`


### examples

```
mysql --host=eu01.pub.bdata.one --port=3301 --user=eosio_activity_ro --password=eosio_activity_ro --database=eosio_activity

show tables;

select count(distinct authorizer) as USERS, sum(counter) as CNT, contract 
from eos_DAILY_COUNTS where xday = '2021-11-09' group by contract  order by CNT desc limit 30;
```
