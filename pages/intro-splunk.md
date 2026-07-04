---
---

# Overview of Splunk

The event flow view

- Indexing flow enables application events to become searchable insight
- Search flow enables fast searches across huge volumes of events and can turns the results into dashboards, alerts, and reports

<img src="../assets/splunk-logo.png" class="absolute top-58 left-93" style="width: 10%; height: auto;"/>

```mermaid
flowchart LR
  A -->|send logs| I
  I -->|parse and store| ST
  U -->|connect to| SearchHead
  U -->|create| SE
  U -->|create| DAR
  DAR -->|from| SE
  SE -->|retrieve| ST

  subgraph SplunkUsers[" "]
    A["🖥️ Applications"]
    U["🧑‍💻 Users"]
  end

  subgraph Splunk[" "]
    IndexerCluster
    SearchHeadCluster
  end

  subgraph IndexerCluster
    I[Forwarders and Indexers]
  end

  subgraph SearchHeadCluster
    SearchHead
  end

  subgraph SearchHead
    SE[Searches]
    DAR[Dashboards, Alerts, Reports]
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

---
---

# Searchhead view of Splunk \[TBC\]

The user entrypoint

- Put screenshots of
  - Search UI
  - Dashboard UI
  - Alert UI
- We are going to use this view for our demos