---
layout: section
---

# How to reduce costs before storing

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

# Implement your own filtering and extraction logic \[TBA\]

Change type: Splunk configuration change

<div class="grid grid-cols-2 gap-6 mt-8 text-left">
  <div class="p-4 rounded-xl border border-blue-400/40 bg-blue-500/10">
    <h3 class="font-semibold mb-2">props.conf</h3>
    <ul class="text-sm leading-7">
      <li>Create event and field manipulation <strong>rules</strong></li>
    </ul>
  </div>
  <div class="p-4 rounded-xl border border-yellow-400/40 bg-yellow-500/10">
    <h3 class="font-semibold mb-2">transforms.conf</h3>
    <ul class="text-sm leading-7">
      <li>Apply manipulation <strong>rules</strong></li>
      <li><strong>Route</strong> events where needed</li>
    </ul>
  </div>
</div>

::left::

```ini
[stanza 1]
a = b

[stanza 2]
c = d
```

::right::

```ini
[stanza 1]
a = b

[stanza 2]
c = d
```
<!-- 
- Use `props.conf` and  `transforms.conf` to:
  - control the flow of events
  - enrich them
  - filter them before they reach Splunk.
   -->