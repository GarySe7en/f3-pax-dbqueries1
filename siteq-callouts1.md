```sql
set @AO = 'ao_iron_lion';

SELECT PAX
, @AO as AO
, SUM(case when ( ao=@AO ) then 1 else 0 end) as AO_count
, max(case when ( ao=@AO ) then date else 0 end) as AO_last
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 1 month ) and date <= ( now() ) ) THEN 1 ELSE 0 END) as 1_month
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 2 month ) and date <= ( now() - INTERVAL 1 month ) ) THEN 1 ELSE 0 END) as 2_months
, SUM(CASE WHEN ( ao=@AO and date > ( now() - INTERVAL 3 month ) and date <= ( now() - INTERVAL 2 month ) ) THEN 1 ELSE 0 END) as 3_months
, max(date) as last_post
, count(*) as total_posts
from f3naperville.attendance_view
where date > ( now() - INTERVAL 3 month)
group by PAX
order by AO_last desc, pax asc
```
