---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Reduce Log Ingestion Costs With Pattern Detection in Splunk
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 35min
---

# Reduce Log Ingestion Costs

with Pattern Detection in Splunk

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# Title, abstract and talk info

**Reduce Log Ingestion Costs With Pattern Detection in Splunk**

By Michele Caci

Snorkeling Session (25 min) on Wednesday 8 July 16:40 - 17:05

Amphi 139 (160 places)

This session presents a Splunk dashboard designed to analyze log behavior, reveal high‑volume patterns, and highlight opportunities to reduce ingestion costs. Participants will learn how to identify repetitive noise, detect inefficient logging, and focus on events that matter. The session provides practical techniques to optimize log volume, improve observability, and support data‑driven decisions.

---
transition: fade-out
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

---
layout: statement
---

# We have a logging problem

We log a lot

---
layout: statement
---

# I mean we really log a lot

We're talking PBs here

---
layout: statement
---

# And if you're here you probably log a lot too

---
layout: intro
---

# 👋 Hello

Who am I?

- I'm Michele
- I deploy and operate the Amadeus logging infrastructure to help applications detect issues from logs
- My hobbies include languages, board games and silly GIFs with Go

<br/>

<img v-click src="./assets/micheleRomeo.jpg" class="absolute top-15 right-15" style="width: 30%; height: auto;"/>
<arrow v-after x1="800" y1="305" x2="825" y2="250" color="#F00" width="1" arrowSize="1" />
<p v-after class="absolute bottom-54 right-46 opacity-100 transform -rotate-10" color="#F00">me</p>
<p v-click class="absolute bottom-57 right-50 opacity-100 transform -rotate-10" color="#F00">young</p>
<p v-after class="absolute top-25 right-100 opacity-100 transform -rotate-20" color="#F00">actual me</p>
<arrow v-after x1="580" y1="125" x2="640" y2="125" color="#F00" width="1" arrowSize="1" />
<img v-click src="./assets/amadeus-workmark_DarkSky.png" class="absolute bottom-15 left-10" style="width: 30%; height: auto;"/>
<img v-after src="./assets/splunk-logo.png" class="absolute bottom-10 left-15" style="width: 10%; height: auto;"/>
<p v-click class="absolute bottom-15 left-80 opacity-100 transform -rotate-350" color="#F00">こんにちわ！</p>
<p v-after class="absolute bottom-3 left-83 opacity-100 transform -rotate-10" color="#F00">Bom dia!  </p>
<img v-click src="./assets/TTR_USA_map_graph.jpg" class="absolute bottom-2 left-115" style="width: 24%; height: auto;"/>
<img v-click src="./assets/picasso-gopher.gif" class="absolute bottom-2 right-20" style="width: 15%; height: auto;"/>

---
layout: fact
---

# 80% of your logs are never read


---
layout: statement
---

# High volume of logs == low signal to noise ratio


---
layout: statement
---

# High volume of logs == high costs

---
transition: fade-out
---

# Splunk provides tools to show you places where you can cut logs

---
transition: fade-out
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
transition: fade-out
---

# How events flow in Splunk

<div class="text-sm text-gray-300 mb-4">From application logs to searchable insight in a few steps</div>

```mermaid
flowchart LR
  A["🖥️ Applications"] -->|emit| B[Forwarders]
  B -->|collect| C[Indexers]
  C -->|store| S["🗄️ Indexed events"]
  D["🧑‍💻 Users"] -->|connect to| E[Search Head]
  E -->|retrieve| S
  E -->|build| F[Dashboards]
  E -->|trigger| G[Alerts]

  subgraph SplunkZone["🧰 Splunk environment"]
    B
    C
    E
    F
    G
  end

  style A fill:#fff7ed,stroke:#fb923c,stroke-width:2px
  style SplunkZone fill:#eff6ff,stroke:#60a5fa,stroke-width:2px
  style D fill:#f6ffef,stroke:#a5fa60,stroke-width:2px
  style S fill:#fef3c7,stroke:#f59e0b,stroke-width:2px
```

<div class="mt-4 text-left text-sm leading-7">
  <div>📤 Applications emit logs</div>
  <div>📦 Splunk ingests and indexes them</div>
  <div>🧑‍💻 Users run searches, dashboards, and alerts</div>
</div>

---
transition: fade-out
---

# 1. Start with the right scope

<div class="grid grid-cols-[1.2fr_0.8fr] gap-6 mt-8 text-left items-start">
  <ul class="text-xl leading-10">
    <li>🧭 Choose the right index and time range first</li>
    <li>🧩 Split data by app, component, phase, or event type</li>
    <li>🎯 Focus on the highest-volume sources before touching everything</li>
  </ul>
  <div class="p-4 rounded-xl border border-amber-400/40 bg-amber-500/10">
    <div class="text-sm font-semibold mb-2">SPL template</div>
    <pre class="text-xs leading-6"><code>index=your_index earliest=-1h latest=now
| stats count by host, source</code></pre>
  </div>
</div>

---
transition: fade-out
---

# 2. Group by transaction

<div class="grid grid-cols-[1.2fr_0.8fr] gap-6 mt-8 text-left items-start">
  <ul class="text-xl leading-10">
    <li>🔗 Use transaction IDs or correlation IDs when available</li>
    <li>📊 Count events per transaction to reveal noisy flows</li>
    <li>⚠️ Spot outliers: a small number of transactions may explain most of the volume</li>
  </ul>
  <div class="p-4 rounded-xl border border-emerald-400/40 bg-emerald-500/10">
    <div class="text-sm font-semibold mb-2">SPL template</div>
    <pre class="text-xs leading-6"><code>index=your_index
| stats count by transaction_id
| sort -count</code></pre>
  </div>
</div>

---
transition: fade-out
---

# 3. Look for repeated patterns

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
transition: fade-out
---

# 4. Turn findings into action

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

---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
