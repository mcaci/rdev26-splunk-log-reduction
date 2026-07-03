---
---

# Overview of Splunk

<div class="grid grid-cols-2 gap-6 mt-8 text-left">
  <div class="p-4 rounded-xl border border-orange-400/40 bg-orange-500/10">
    <div class="text-3xl mb-2">📱💻</div>
    <h3 class="font-semibold mb-2">Applications and users</h3>
    <ul class="text-sm leading-7">
      <li>Applications generate log events continuously</li>
      <li>Users and operators need answers quickly</li>
      <li>Logs are the raw signal for troubleshooting and monitoring</li>
    </ul>
  </div>
  <div class="p-4 rounded-xl border border-sky-400/40 bg-sky-500/10">
    <div class="text-3xl mb-2">🧠🔎</div>
    <h3 class="font-semibold mb-2">The Splunk platform</h3>
    <ul class="text-sm leading-7">
      <li>Collects, parses, stores, and indexes the data</li>
      <li>Enables fast searches across huge volumes of events</li>
      <li>Turns raw logs into dashboards, alerts, and reports</li>
    </ul>
  </div>
</div>

---
---

# How events flow in Splunk


<div class="text-sm text-gray-300 mb-4">From application logs to searchable insight in a few steps</div>

<!--
  A["🖥️ Applications"] -/->|send logs| B[Forwarders] 
  B -/->|collect| C[Indexers]
-->

```mermaid
flowchart LR
  A -->|send logs| I
  I -->|parse and store| ST
  U -->|connect to| SearchHead
  U -->|create| SE
  SE -->|retrieve| ST

  subgraph SplunkUsers[" "]
    A["🖥️ Applications"]
    U["🧑‍💻 Users"]
  end

  subgraph Splunk
    IndexerCluster
    SearchHeadCluster
  end

  subgraph IndexerCluster
    I[Indexers]
  end

  subgraph SearchHeadCluster
    SearchHead
  end

  subgraph SearchHead
    SE[Searches]
  end

  subgraph Storage["Storage"]
    ST["🗄️ Indexed events"]
  end

  style A fill:#fff7ed,stroke:#fb923c,stroke-width:2px
  style Splunk fill:#eff6ff,stroke:#60a5fa,stroke-width:2px
  style SplunkUsers fill:#ffffff,stroke:#ffffff,stroke-width:2px
  style U fill:#f6ffef,stroke:#a5fa60,stroke-width:2px
  style ST fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
```

<img src="./assets/splunk-logo.png" class="absolute top-28 left-125" style="width: 10%; height: auto;"/>
