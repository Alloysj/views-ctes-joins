## Inspect the yum Table (62)
```PRAGMA table_info(yum);````

verify
```SELECT * FROM yum LIMIT 5;```

## Aggregate `yum` Data by Month and Year (63)
```
SELECT 
    strftime('%Y', date) AS year,
    strftime('%m', date) AS month,
    AVG(open) AS avg_open,
    AVG(high) AS avg_high,
    AVG(low) AS avg_low,
    AVG(close) AS avg_close,
    SUM(volume) AS total_volume
FROM yum
GROUP BY year, month
ORDER BY year, month;
```

## Save as a View (64)
```
CREATE VIEW yum_by_month AS
SELECT 
    strftime('%Y', date) AS year,
    strftime('%m', date) AS month,
    AVG(open) AS avg_open,
    AVG(high) AS avg_high,
    AVG(low) AS avg_low,
    AVG(close) AS avg_close,
    SUM(volume) AS total_volume
FROM yum
GROUP BY year, month;
```

### Check if view is created 
`SELECT * FROM yum_by_month LIMIT 5;`

