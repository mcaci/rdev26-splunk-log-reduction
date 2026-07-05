---
layout: section
---

# How to reduce costs after storing

---
layout: fact
---

# Splunk Queries

---
---

# Query #1: General view of how your application logs: DEMO!

- Get the event count

```spl
| tstats count
    WHERE index=your_index_1 OR index=your_index_2
    BY index
```

- More fields for a more granular view

```spl
    BY index, remote_host, status, source
| sort - count
```

- Focus the event analysis on the highest-volume sources.
- DEMO or ADD screenshots

<!-- 
before touching anything
-->

---
---

# Query #2: Look for high events count on transaction: DEMO!

- Get a list of transactions grouped by event count

```spl
index=your_index
| stats count by transaction_id
| sort - count
```

- Transactions that log more event than average are a sign of abnormal behavior
- Bonus: Splunk lets you trigger actions on this list

SCREENSHOT of trigger action

- DEMO or ADD screenshots

---
---

#  Query #3: Pattern detection: DEMO!

- Look for exact matches

```spl
index=your_index
| stats count by _raw
| sort -count
```

- By using `props.conf` and `transforms.conf` you can replace `_raw` with a more precise subset

- Look for repeated patterns

```spl
index=your_index
| cluster t=0.9 showcount=true
| table cluster_count _raw
| sort - cluster_count
```

- Split results further for deeper analysis
- Use them to isolate frequent events and patterns and verify their usefulness
- DEMO or ADD screenshots

---
layout: fact
---

# Save the Queries

---
---

# Turn findings into actionable items

Alerts and dashboards

- Queries do not watch themselves
- Create alerts to be notified for:
  - transactions detected with more than `x` events.
  - patterns repeating more than `y` times.
- Create dashboards that monitor the results of these queries.

<br/>

- Alerts and dashboards provide:
  - A persistent place to visualize the result of your queries
  - A regular watch on the state of your logs
- DEMO or ADD screenshots
<!-- 
You will not go and connect to splunk from time to time to check these results.
Let Splunk do it for you.

And call the splunk application containing them "log_analyzer"
 -->