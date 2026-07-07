---
layout: section
---

# How to make savings at indexing

<!-- 
Make Splunk's life easy by helping it detect and extract fields
-->

---
layout: two-cols-header
---

# Migrate from unstructured to structured logging

Change type: Application code change

<div class="grid grid-cols-2 gap-6 mt-8 text-left">
  <div class="p-4 rounded-xl border border-orange-400/40 bg-orange-500/10">
    <h3 class="font-semibold mb-2">Unstructured Logs</h3>
    <ul class="text-sm leading-7">
      <li>Event is a free text to be parsed</li>
      <li>Event field extraction is <strong>delegated</strong> to Splunk</li>
      <li><strong>High CPU usage</strong> on Splunk indexing flow</li>
    </ul>
  </div>
  <div class="p-4 rounded-xl border border-sky-400/40 bg-sky-500/10">
    <h3 class="font-semibold mb-2">Structured Logs</h3>
    <ul class="text-sm leading-7">
      <li><strong>Minimize</strong> the event <strong>payload</strong>, <strong>maximize</strong> event <strong>field</strong> usage</li>
      <li>Event field extraction is easier on Splunk</li>
      <li><strong>Save CPU usage</strong> on Splunk indexing flow</li>
      <li>Indexing is agnostic to event structure</li>
    </ul>
  </div>
</div>

::left::

`2026/07/02 23:00:14 measurements ok remote address is 192.168.1.99:3041 temperature is 29.4C humidity is 70% RH`

::right::

`time=2026-07-02T23:00:14.960+02:00 level=INFO msg="measurements ok" remote=192.168.1.99:3041 temperature=29.4 temperature_unit=C humidity=70% humidity_unit=RH`

---
hide: true
---

# Implement your own filtering and extraction logic

Change type: Splunk config change

<div class="grid grid-cols-2 gap-6 mt-8 text-left">
  <div class="p-4 rounded-xl border border-blue-400/40 bg-blue-500/10">
    <h3 class="font-semibold mb-2">props.conf</h3>
    <ul class="text-sm leading-7">
      <li>Configure index and search time <strong>field extraction rules</strong></li>
      <li>Regex-based customization of sourcetype and host fields</li>
      <li>Customize anonymization rules for sensitive data</li>
    </ul>
  </div>
  <div class="p-4 rounded-xl border border-yellow-400/40 bg-yellow-500/10">
    <h3 class="font-semibold mb-2">transforms.conf</h3>
    <ul class="text-sm leading-7">
      <li>Apply field extraction <strong>rules</strong></li>
      <li>Customize <strong>event routing</strong>, even dropping if needed</li>
    </ul>
  </div>
</div>

Use `props.conf` and  `transforms.conf` to control the flow of events, enrich them and/or filter them before they reach splunk.

---
layout: two-cols-header
---

# Implement your own filtering and extraction logic

Change type: Splunk configuration change

<div class="grid grid-cols-[0.85fr_1.05fr] gap-5 mt-8 text-left">
  <div class="p-4 rounded-xl border border-blue-400/40 bg-blue-500/10">
    <h3 class="font-semibold mb-2">props.conf</h3>
    <ul class="text-sm leading-7">
      <li>Create event and field manipulation <strong>rules</strong></li>
      <li>Create routing <strong>rules</strong></li>
    </ul>
  </div>
  <div class="p-4 rounded-xl border border-yellow-400/40 bg-yellow-500/10" style="width: 85%">
    <h3 class="font-semibold mb-2">transforms.conf</h3>
    <ul class="text-sm leading-7">
      <li>Apply props.conf manipulation <strong>rules</strong></li>
      <li><strong>Route</strong> events according to props.conf rules</li>
    </ul>
  </div>
</div>

<br/>

::left::

```ini
[sensor_transactions]
# Timestamp parsing — matches "2026/07/02 23:00:14"
TIME_FORMAT         = %Y/%m/%d %H:%M:%S
TIME_PREFIX         = ^\s*
# Field extractions (search-time, via transforms.conf)
TRANSFORMS-sensor_fields = extract_sensor_fields
# Indexing routing (evaluated in order, first match wins)
# Both transforms are defined in transforms.conf
TRANSFORMS-routing  = route_ok_to_sensor_ok,\
route_errors_to_sensor_errors
# Auto-assign sourcetype by file path
[source::.../sensor_*.log]
sourcetype = sensor_transactions
```

::right::

```ini
[extract_sensor_fields]
REGEX = ^\d{4}/\d{2}/\d{2}\s+\d{2}:\d{2}:\d{2}\s+(?P<transaction_id>\S+)...
WRITE_META = true # field extraction at index time

[route_ok_to_sensor_ok]
REGEX   = (?i)\b\w+\s+ok\b
DEST_KEY = _MetaData:Index
FORMAT  = sensor_ok

[route_errors_to_sensor_errors]
REGEX   = (?i)(?:error|fail|timeout|refused|denied|critical|alert|warn)
DEST_KEY = _MetaData:Index
FORMAT  = sensor_errors
```

<!-- 
- Use `props.conf` and  `transforms.conf` to:
  - control the flow of events
  - enrich them
  - filter them before they reach Splunk.

Full stanza:
REGEX = ^\d{4}/\d{2}/\d{2}\s+\d{2}:\d{2}:\d{2}\s+(?P<transaction_id>\S+)\s+(?P<status>[a-zA-Z][^\d]*?(?=remote|$))\s*(?:remote address is\s+(?P<remote_host>[\d.]+):(?P<remote_port>\d+))?(?:.*?temperature is\s+(?P<temperature>[\d.]+)C)?(?:.*?humidity is\s+(?P<humidity>\d+)%)?(?:.*?code is\s+(?P<error_code>\S+))?
-->