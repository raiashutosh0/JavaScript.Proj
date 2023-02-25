SELECT 
    item_date, 
    slot, 
    DATE_TRUNC('day', schedule_time) AS scheduling_day, 
    AVG(EXTRACT(EPOCH FROM schedule_time - DATE_TRUNC('day', schedule_time))) AS scheduling_time 
FROM orders 
GROUP BY 
    item_date, 
    slot, 
    scheduling_day 
ORDER BY 
    item_date, 
    slot;
