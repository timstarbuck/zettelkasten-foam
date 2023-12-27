# KQL Notes and Queries

Example of summary query by timestamp (by day in this case)
```
pageViews
| summarize PageViewCount = count() by bin(timestamp, 1d)
| render timechart 
```

Example of summary query - AVG CPU
```
performanceCounters
| where category == 'Processor' and name == '% Processor Time'
| summarize CPU = avg(value) by bin(timestamp, 15m)
| render timechart 
```
