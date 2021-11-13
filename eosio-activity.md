# Daily activity database for public EOSIO chains

Database schema and writer: [github.com/cc32d9/eosio_activity_db](https://github.com/cc32d9/eosio_activity_db)

Public access:

```
mysql --host=eu01.pub.bdata.one --port=3301 --user=eosio_activity_ro --password=eosio_activity_ro --database=eosio_activity

show tables;

select count(distinct authorizer) as USERS, sum(counter) as CNT, contract from eos_DAILY_COUNTS where xday = '2021-11-09' group by contract  order by CNT desc limit 30;
```
