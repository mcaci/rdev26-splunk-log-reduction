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

# Query #1: Topology search and event count: DEMO!

Get a general view of how your application logs

Get a count of the events in Splunk in a specific time window

```spl
| tstats count
    WHERE index=sensor_ok OR index=sensor_errors
    BY index
```

do you have fields categorizing your application (app_component, app_backend, phase, event_type)? Group them!

```spl
| tstats count
    WHERE (index=sensor_ok OR index=sensor_errors)
    BY index, remote_host, status, source
| sort - count
```

- Choose the right index and time range first
- Split data by app, component, phase or event type
- Focus on the highest-volume sources before touching everything

---
---

# Query #2: Transaction search

Lookout for high count on transaction events



<div class="grid grid-cols-[1.2fr_0.8fr] gap-6 mt-8 text-left items-start">
  <ul class="text-xl leading-10">
    <li>Use transaction IDs or correlation IDs when available</li>
    <li>Count events per transaction to reveal noisy flows</li>
    <li>Spot outliers: a small number of transactions may explain most of the volume</li>
  </ul>
  <div class="p-4 rounded-xl border border-emerald-400/40 bg-emerald-500/10">
    <div class="text-sm font-semibold mb-2">SPL template</div>
    <pre class="text-xs leading-6"><code>index=your_index
| stats count by transaction_id
| sort -count</code></pre>
  </div>
</div>

---
---

#  Query #3: Pattern detection

Look for repeated patterns

<div class="grid grid-cols-[1.2fr_0.8fr] gap-6 mt-8 text-left items-start">
  <ul class="text-xl leading-10">
    <li>🔎 Search for exact repeated messages and frequent values</li>
    <li>🔁 Watch for retries, boilerplate logs, and duplicate events</li>
    <li>🧠 Use <code>stats</code>, <code>top</code>, <code>rare</code>, and <code>collect</code> to uncover patterns</li>
  </ul>
  <div class="p-4 rounded-xl border border-sky-400/40 bg-sky-500/10">
    <div class="text-sm font-semibold mb-2">SPL template</div>
    <pre class="text-xs leading-6"><code>index=your_index
| stats count by message
| sort -count
| where count > 100</code></pre>
  </div>
</div>

---
layout: fact
---

# Save the Queries

---
---

# Turn findings into action

Turn and persist as dashboards and alerts are ways to concreting queries into viz and actions

<div class="grid grid-cols-[1.2fr_0.8fr] gap-6 mt-8 text-left items-start">
  <ul class="text-xl leading-10">
    <li>📈 Build a dashboard, saved search, or alert to keep the signal visible</li>
    <li>🛑 Decide whether to suppress, sample, or redesign noisy logging</li>
    <li>✅ Measure the impact and iterate as the system evolves</li>
  </ul>
  <div class="p-4 rounded-xl border border-violet-400/40 bg-violet-500/10">
    <div class="text-sm font-semibold mb-2">SPL template</div>
    <pre class="text-xs leading-6"><code>index=your_index
| stats count by message
| where count > 1000
| eval action="review logging"</code></pre>
  </div>
</div>