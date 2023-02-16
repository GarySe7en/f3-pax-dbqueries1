This query checks the attendance database and counts how many times a PAX has attended a specific AO over the past 3 months. Compare that with total attendence. See who are your regulars, who may be posting regularly but at another AO, and who needs some encouragement to come out and post again.

```sql
set @AO = 'ao_NAME_HERE' ;
set @region_db = 'f3naperville' ;

SELECT PAX
, @AO as AO
, SUM(case when ( ao=@AO ) then 1 else 0 end) as AO_count
, max(case when ( ao=@AO ) then date else 0 end) as AO_last
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 1 month ) and date <= ( now() ) ) THEN 1 ELSE 0 END) as 1_month
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 2 month ) and date <= ( now() - INTERVAL 1 month ) ) THEN 1 ELSE 0 END) as 2_months
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 3 month ) and date <= ( now() - INTERVAL 2 month ) ) THEN 1 ELSE 0 END) as 3_months
, max(date) as last_post
, count(*) as total_posts
from @region_db.attendance_view
where date > ( now() - INTERVAL 3 month)
group by PAX
order by AO_last desc, pax asc
limit 100
```
