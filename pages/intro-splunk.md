---
layout: two-cols
hide: true
---

# The DevOps View
## Where cost is born

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    Your leverage is <strong class="text-white">before</strong> data hits
    the indexer — filter, sample, or drop at the forwarder.
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
    <div class="text-blue-300 font-semibold text-sm">inputs.conf</div>
    <div class="text-gray-500 text-xs mt-1">Controls what the forwarder collects — file paths, ports, scripts</div>
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
    <div class="text-blue-300 font-semibold text-sm">props.conf + transforms.conf</div>
    <div class="text-gray-500 text-xs mt-1">Filter, route, or drop events <span class="text-yellow-400">before indexing</span> — the most cost-effective lever</div>
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
    <div class="text-blue-300 font-semibold text-sm">indexes.conf</div>
    <div class="text-gray-500 text-xs mt-1">Define retention and routing per index — align with data criticality</div>
  </div>

  <div class="bg-yellow-950/30 border border-yellow-500/20 rounded-lg px-4 py-3 text-xs text-yellow-200/80">
    💡 Dropping at the forwarder costs nothing. Dropping after indexing saves nothing.
  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500"># props.conf</div>
    <div class="text-gray-300">[source::/var/log/myapp.log]</div>
    <div class="text-blue-300">TRANSFORMS-drop = drop-debug-lines</div>
    <br/>
    <div class="text-gray-500"># transforms.conf</div>
    <div class="text-gray-300">[drop-debug-lines]</div>
    <div class="text-blue-300">REGEX = \sDEBUG\s</div>
    <div class="text-blue-300">DEST_KEY = queue</div>
    <div class="text-purple-400">VALUE = nullQueue</div>
  </div>

  <div class="text-gray-600 text-xs text-center">
    nullQueue = discarded before indexing
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs text-gray-400">
    <div class="text-white font-semibold mb-2">Typical index layout</div>
    <div class="grid grid-cols-2 gap-x-4 gap-y-1">
      <div>📁 <span class="text-blue-300">app_audit</span></div>
      <div class="text-gray-600">compliance, long retention</div>
      <div>📁 <span class="text-blue-300">app_functional</span></div>
      <div class="text-gray-600">business events</div>
      <div>📁 <span class="text-blue-300">app_monitoring</span></div>
      <div class="text-gray-600">metrics, health checks</div>
      <div>📁 <span class="text-blue-300">app_debug</span></div>
      <div class="text-red-400">💸 biggest, least read</div>
    </div>
  </div>

</div>

---
---

# Overview of Splunk

- __Indexing flow__ enables application events to become searchable insight
- __Search flow__ enables fast searches across huge volumes of events

<br/>

<img src="../assets/splunk-logo-w.png" class="absolute top-50 left-93" style="width: 10%; height: auto;"/>

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

  classDef app fill:#1f2937,stroke:#fb923c,stroke-width:2px,color:#fff7ed
  classDef user fill:#14532d,stroke:#86efac,stroke-width:2px,color:#ecfdf5
  classDef core fill:#172554,stroke:#60a5fa,stroke-width:2px,color:#e0f2fe
  classDef storage fill:#78350f,stroke:#f59e0b,stroke-width:2px,color:#fffbeb
  classDef group fill:#111827,stroke:#e5e7eb,stroke-width:1px,color:#f9fafb

  class A app
  class U user
  class I,SearchHead,SE,DAR core
  class ST storage
  class Splunk,SplunkUsers,IndexerCluster,SearchHeadCluster,Storage group

  linkStyle default stroke:#94a3b8,stroke-width:2px
```

<div class="mx-auto mt-4 flex items-center justify-center text-center w-fit border border-yellow-500/20 rounded-lg bg-yellow-950/30 px-4 py-3 text-xs text-yellow-200/80">
  💡 Savings are made at indexing time. At search time you are left with cost analysis.
</div>

---
layout: default
hide: true
---

# Splunk at a Glance

<div class="grid grid-cols-2 gap-6 mt-6">

  <!-- Left: DevOps / Ingestion view -->
  <div class="flex flex-col gap-3">
    <div class="text-xs uppercase tracking-widest text-blue-400 font-semibold mb-1">
      🔧 DevOps View — How logs get in
    </div>

    <!-- Pipeline diagram -->
    <div class="flex flex-col gap-2 font-mono text-xs">

      <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-center gap-3">
        <span class="text-2xl">🖥️</span>
        <div>
          <div class="text-white font-semibold">Applications / Services</div>
          <div class="text-gray-500">emit logs to stdout, files, or syslog</div>
        </div>
      </div>

      <div class="text-center text-gray-600 text-lg">↓</div>

      <div class="bg-white/5 border border-blue-500/20 rounded-lg px-4 py-3 flex items-center gap-3">
        <span class="text-2xl">🚚</span>
        <div>
          <div class="text-white font-semibold">Forwarder</div>
          <div class="text-gray-500">Universal Forwarder · HEC · Syslog</div>
        </div>
      </div>

      <div class="text-center text-gray-600 text-lg">↓</div>

      <div class="bg-white/5 border border-blue-500/20 rounded-lg px-4 py-3 flex items-center gap-3">
        <span class="text-2xl">⚙️</span>
        <div>
          <div class="text-white font-semibold">Indexer</div>
          <div class="text-gray-500">parses, indexes, stores — <span class="text-red-400">this is what you pay for</span></div>
        </div>
      </div>

      <div class="text-center text-gray-600 text-lg">↓</div>

      <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-center gap-3">
        <span class="text-2xl">🗄️</span>
        <div>
          <div class="text-white font-semibold">Indexes</div>
          <div class="text-gray-500">partitioned by app / phase / event type</div>
        </div>
      </div>

    </div>
  </div>

  <!-- Right: User / Analyst view -->
  <div class="flex flex-col gap-3">
    <div class="text-xs uppercase tracking-widest text-purple-400 font-semibold mb-1">
      👤 User View — How you interact with it
    </div>

    <div class="flex flex-col gap-2">

      <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-start gap-3">
        <span class="text-xl mt-0.5">🗂️</span>
        <div>
          <div class="text-white text-sm font-semibold">Indexes</div>
          <div class="text-gray-500 text-xs leading-relaxed">
            Your entry point. One app may write to several indexes:
            audit, functional, monitoring, debug…
          </div>
        </div>
      </div>

      <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-start gap-3">
        <span class="text-xl mt-0.5">🔎</span>
        <div>
          <div class="text-white text-sm font-semibold">Search (SPL)</div>
          <div class="text-gray-500 text-xs leading-relaxed">
            Splunk's query language. The tool we'll use today
            to surface patterns and outliers.
          </div>
        </div>
      </div>

      <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-start gap-3">
        <span class="text-xl mt-0.5">📋</span>
        <div>
          <div class="text-white text-sm font-semibold">Saved Searches & Alerts</div>
          <div class="text-gray-500 text-xs leading-relaxed">
            Schedule queries to run automatically.
            Useful for ongoing volume monitoring.
          </div>
        </div>
      </div>

      <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 flex items-start gap-3">
        <span class="text-xl mt-0.5">📊</span>
        <div>
          <div class="text-white text-sm font-semibold">Dashboards</div>
          <div class="text-gray-500 text-xs leading-relaxed">
            We'll build one of these by the end — a live view
            of your log volume, patterns, and top offenders.
          </div>
        </div>
      </div>

    </div>
  </div>

</div>