---
---

# Title, abstract and talk info

**Reduce Log Ingestion Costs With Pattern Detection in Splunk**

By Michele Caci

Snorkeling Session (25 min) on Wednesday 8 July 16:40 - 17:05

Amphi 139 (160 places)

This session presents a Splunk dashboard designed to analyze log behavior, reveal high‑volume patterns, and highlight opportunities to reduce ingestion costs. Participants will learn how to identify repetitive noise, detect inefficient logging, and focus on events that matter. The session provides practical techniques to optimize log volume, improve observability, and support data‑driven decisions.

---
---

# Outline

Time breakdown idea: **Intro .1-.4**: 5 minutes; **Splunk .5**: 5 minutes; **How .6**: 10 minutes; **Q/A**: 5 minutes

1. Self-intro
2. Introduction: we (amadeus) log a lot, really a lot (vague order of magnitude) and you do too.
3. Problem statement: Many logs = high costs = low signal/noise ratio, 80% of logs are not read (link quote)
4. Main point: if you are using splunk I will show techniques you can use to find ways to reduce logs
5. Splunk overview: high-level architecture + devops view (deployment and log ingestion) vs user view (index creation, searches, splunk application/dashboards and saved searches)
6. What to analyze and how:
- which index to look at (an app can usually log to different indexes according to phase/app component/event type(audit,classic,functional monitoring,...))
- Logging by transaction: if not you should. group and count logs by trx_id. find outliers (transaction with high count)
- Patterns: queries for exact matches and for finding patterns `collect` command and others.
- Put all in a dashboard