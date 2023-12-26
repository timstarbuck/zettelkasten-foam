# KQL Notes and Queries

Example of summary query by timestamp (by day in this case)
```
pageViews
| summarize PageViewCount = count() by bin(timestamp, 1d)
| render timechart 
```
