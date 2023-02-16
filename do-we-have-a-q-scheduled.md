This query is for the siteQs to confirm a Q is scheduled, so they can EH someone as needed--or plan a workout.

* AO_CID - the AO channel ID that qsignups uses.
* AO_DOW - day of week to check for
  * 1 - Sunday
  * 2 - Monday
  * 3 - Tuesday
  * 4 - Wednesday
  * 5 - Thursday
  * 6 - Friday
  * 7 - Saturday
* weeks - how many weeks out to report on

```sql
set @AO_CID = 'C02NH9HJ1RD';
set @AO_DOW = 5 ;
set @weeks = 3 ;

select 
 q_pax_name
, event_date
, event_time
, event_type
, event_day_of_week
from f3stcharles.qsignups_master
where ao_channel_id = @AO_CID
and event_date > now()
and event_date < ( now() + INTERVAL (@weeks*7+1) day)
and dayofweek(event_date)=@AO_DOW
order by event_date asc 
```
