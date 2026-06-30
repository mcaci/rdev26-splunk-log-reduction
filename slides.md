---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /images/glowing-digital-interface-stockcake.jpg
colorSchema: dark
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
## with Pattern Detection in Splunk

<div class="absolute bottom-10 text-left">
    <div>Michele Caci</div>
    <div>Senior Software Engineer @ Amadeus</div>
    <div class="flex m-0 gap-1">
      <a href="https://github.com/mcaci" target="_blank" alt="Michele's GitHub" title="Michele's GitHub"
        class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
        <carbon-logo-github />
      </a>
      <a href="https://www.linkedin.com/in/michele-caci-47770132/" target="_blank" alt="Michele's Linkedin" title="Michele's Linkedin"
        class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
        <carbon-logo-linkedin />
      </a>
    </div>
</div>

---
src: ./pages/intro-talkoutline.md
hide: false
---

---
src: ./pages/problem-statement-intro.md
hide: false
---

---
src: ./pages/whoami.md
hide: false
---

# We log too much
<v-click>

## And if you're here, you probably log too much too
</v-click>

---
layout: center
---

<style>
  @keyframes scroll-up {
  from { transform: translateY(0); }
  to   { transform: translateY(-50%); }
}
.log-scroll {
  animation: scroll-up 12s linear infinite;
}
</style>

<div class="grid grid-cols-2 gap-8 mt-6 h-80">

<!-- Left: the numbers -->
<div class="flex flex-col justify-around">

  <div>
    <div class="text-6xl font-bold text-blue-400 tracking-tight">~50 TB</div>
    <div class="text-gray-400 text-sm mt-1">of logs ingested per day at Amadeus</div>
  </div>

  <div>
    <!-- <div class="text-6xl font-bold text-indigo-400 tracking-tight">10<span class="text-3xl align-super font-normal">9</span></div> -->
    <div class="text-6xl font-bold text-indigo-400 tracking-tight">1 billion</div>
    <div class="text-gray-400 text-sm mt-1">events generated daily</div>
  </div>

  <div>
    <!-- <div class="text-6xl font-bold text-purple-400 tracking-tight">100s</div> -->
    <div class="text-6xl font-bold text-purple-400 tracking-tight">Hudreds</div>
    <div class="text-gray-400 text-sm mt-1">of applications, pipelines, and services</div>
  </div>

</div>

<!-- Right: the stream -->
<div class="relative overflow-hidden rounded-xl border border-white/10 bg-black/40 font-mono text-xs text-green-400/70 p-4 leading-5 select-none">
  <div class="absolute inset-0 bg-gradient-to-b from-transparent via-transparent to-black/80 z-10 pointer-events-none"/>
  <div class="opacity-60 log-scroll">
    2026-06-30T08:42:01.003Z INFO  [booking-svc]    trx=8f3a2 Request received POST /orders<br/>
    2026-06-30T08:42:01.004Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=main<br/>
    2026-06-30T08:42:01.006Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=2ms<br/>
    2026-06-30T08:42:01.008Z INFO  [booking-svc]    trx=8f3a2 Validating passenger data fields=12<br/>
    2026-06-30T08:42:01.009Z DEBUG [validator]      trx=8f3a2 Field check: name OK<br/>
    2026-06-30T08:42:01.009Z DEBUG [validator]      trx=8f3a2 Field check: dob OK<br/>
    2026-06-30T08:42:01.010Z DEBUG [validator]      trx=8f3a2 Field check: passport OK<br/>
    2026-06-30T08:42:01.011Z DEBUG [cache]          trx=8f3a2 Cache miss key=passenger:8f3a2<br/>
    2026-06-30T08:42:01.013Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=main<br/>
    2026-06-30T08:42:01.014Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=1ms<br/>
    2026-06-30T08:42:01.021Z INFO  [pricing-svc]    trx=8f3a2 Fetching fare rules origin=NCE<br/>
    2026-06-30T08:42:01.023Z DEBUG [http-client]    trx=8f3a2 GET /fare-rules timeout=5000ms<br/>
    2026-06-30T08:42:01.038Z DEBUG [http-client]    trx=8f3a2 Response 200 latency=15ms<br/>
    2026-06-30T08:42:01.039Z DEBUG [cache]          trx=8f3a2 Cache miss key=fare:NCE:Y<br/>
    2026-06-30T08:42:01.041Z INFO  [booking-svc]    trx=8f3a2 Seat availability check flight=AF123<br/>
    2026-06-30T08:42:01.055Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=replica<br/>
    2026-06-30T08:42:01.056Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=1ms<br/>
    2026-06-30T08:42:01.071Z INFO  [booking-svc]    trx=8f3a2 Booking confirmed ref=AMZ-99812<br/>
    2026-06-30T08:42:01.072Z INFO  [audit-svc]      trx=8f3a2 Audit event BOOKING_CONFIRMED<br/>
    2026-06-30T08:42:01.073Z DEBUG [notify]         trx=8f3a2 Queuing confirmation email<br/>
    2026-06-30T08:42:01.003Z INFO  [booking-svc]    trx=8f3a2 Request received POST /orders<br/>
    2026-06-30T08:42:01.004Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=main<br/>
    2026-06-30T08:42:01.006Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=2ms<br/>
    2026-06-30T08:42:01.008Z INFO  [booking-svc]    trx=8f3a2 Validating passenger data fields=12<br/>
    2026-06-30T08:42:01.009Z DEBUG [validator]      trx=8f3a2 Field check: name OK<br/>
    2026-06-30T08:42:01.009Z DEBUG [validator]      trx=8f3a2 Field check: dob OK<br/>
    2026-06-30T08:42:01.010Z DEBUG [validator]      trx=8f3a2 Field check: passport OK<br/>
    2026-06-30T08:42:01.011Z DEBUG [cache]          trx=8f3a2 Cache miss key=passenger:8f3a2<br/>
    2026-06-30T08:42:01.013Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=main<br/>
    2026-06-30T08:42:01.014Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=1ms<br/>
    2026-06-30T08:42:01.021Z INFO  [pricing-svc]    trx=8f3a2 Fetching fare rules origin=NCE<br/>
    2026-06-30T08:42:01.023Z DEBUG [http-client]    trx=8f3a2 GET /fare-rules timeout=5000ms<br/>
    2026-06-30T08:42:01.038Z DEBUG [http-client]    trx=8f3a2 Response 200 latency=15ms<br/>
    2026-06-30T08:42:01.039Z DEBUG [cache]          trx=8f3a2 Cache miss key=fare:NCE:Y<br/>
    2026-06-30T08:42:01.041Z INFO  [booking-svc]    trx=8f3a2 Seat availability check flight=AF123<br/>
    2026-06-30T08:42:01.055Z DEBUG [db-pool]        trx=8f3a2 Acquiring connection pool=replica<br/>
    2026-06-30T08:42:01.056Z DEBUG [db-pool]        trx=8f3a2 Connection acquired latency=1ms<br/>
    2026-06-30T08:42:01.071Z INFO  [booking-svc]    trx=8f3a2 Booking confirmed ref=AMZ-99812<br/>
    2026-06-30T08:42:01.072Z INFO  [audit-svc]      trx=8f3a2 Audit event BOOKING_CONFIRMED<br/>
    2026-06-30T08:42:01.073Z DEBUG [notify]         trx=8f3a2 Queuing confirmation email<br/>
  </div>
</div>
</div>

---
layout: statement
---

# We pay for those logs

<!-- 
So let's see how we can reduce those costs, but first -->

---
layout: center
class: text-center
---

<div class="flex flex-col items-center gap-6">

<img v-click src="./images/micheleRomeo.jpg" class="w-40 h-30 rounded-full "/>


<div>
  <h1 class="text-4xl font-bold tracking-tight">Michele Caci</h1>
  <p class="text-xl text-gray-400 mt-1">Senior Software Engineer @ Logging team in Amadeus</p>
</div>


<div class="grid grid-cols-4 gap-4 mt-4 text-sm">
  <div class="bg-white/5 rounded-xl px-5 py-3 border border-white/10">
    <div class="text-blue-400 font-semibold">Focus</div>
    <div class="text-gray-300">Logging - OpenTelemetry</div>
  </div>
  <div class="bg-white/5 rounded-xl px-5 py-3 border border-white/10">
    <div class="text-blue-400 font-semibold">Stack</div>
    <div class="text-gray-300">Go · K8S - Splunk</div>
  </div>
  <div class="bg-white/5 rounded-xl px-5 py-3 border border-white/10">
    <div class="text-blue-400 font-semibold">Hobbies</div>
    <div class="text-gray-300">Cooking, Languages, Board Games</div>
  </div>
  <div class="bg-white/5 rounded-xl px-5 py-3 border border-white/10">
    <div class="text-blue-400 font-semibold">Full-time job</div>
    <div class="text-gray-300">Surviving Fatherhood</div>
  </div>
</div>

</div>

---
layout: default
---

# The Problem With Logging Everything

<div class="grid grid-cols-3 gap-5 mt-8">

  <!-- Card 1: Cost -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">💸</div>
    <div class="text-2xl font-bold text-red-400">High Cost</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      Splunk licenses on <strong class="text-white">ingestion volume</strong>.
      Every DEBUG line, every heartbeat, every
      redundant stack trace — you pay for all of it.
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-red-400/80 text-xs font-mono">
      cost = volume × price/GB
    </div>
  </div>

  <!-- Card 2: Low signal -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">📉</div>
    <div class="text-2xl font-bold text-yellow-400">Low Signal</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      When everything is logged, <strong class="text-white">nothing stands out</strong>.
      Finding a real error means wading through
      thousands of irrelevant lines.
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-yellow-400/80 text-xs">
      noise drowns the signal
    </div>
  </div>

  <!-- Card 3: Unread -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">👻</div>
    <div class="text-2xl font-bold text-purple-400">Never Read</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      Studies show <strong class="text-white">~80% of logs</strong> are
      never queried after ingestion. You're storing
      and indexing data nobody will ever look at.
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-purple-400/80 text-xs">
      <a href="https://your-source-link" class="underline opacity-70 hover:opacity-100">
        Source: [your citation here]
      </a>
    </div>
  </div>

</div>

<!-- Bottom stat bar -->
<div v-click class="mt-6 flex items-center gap-6 bg-red-950/30 border border-red-500/20 rounded-xl px-6 py-4">
  <div class="text-4xl font-black text-red-400 shrink-0">80%</div>
  <div class="text-sm text-gray-300 leading-relaxed">
    of ingested logs are <strong class="text-white">never read</strong> — yet you pay to ingest,
    index, and retain every byte. The goal isn't to log less blindly:
    it's to log <strong class="text-white">smarter</strong>, and find what's safe to cut.
  </div>
</div>

---
layout: fact
---

# 80% of your logs are never read
# Don't log less<br/><br/><v-click> Log smart</v-click>

---
layout: center
class: text-center
---

<div class="flex flex-col items-center gap-8">

  <div class="text-gray-400 text-lg tracking-widest uppercase">The good news</div>

  <h1 class="text-5xl font-black leading-tight max-w-2xl">
    You don't need to log less.<br/>
    <span class="text-blue-400">You need to log smarter.</span>
  </h1>

  <div class="text-gray-400 text-base max-w-xl leading-relaxed">
    If your team uses Splunk, you already have everything you need
    to find the noise, understand the patterns, and make
    data-driven decisions about what to cut.
  </div>

  <div v-click class="flex gap-4 mt-2">
    <div class="bg-white/5 border border-white/10 rounded-xl px-5 py-3 text-sm text-gray-300">
      🔍 Find the heavy hitters
    </div>
    <div class="bg-white/5 border border-white/10 rounded-xl px-5 py-3 text-sm text-gray-300">
      📊 Understand the patterns
    </div>
    <div class="bg-white/5 border border-white/10 rounded-xl px-5 py-3 text-sm text-gray-300">
      ✂️ Cut with confidence
    </div>
  </div>

</div>

---
layout: default
---

# Splunk at a Glance

---
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

---
src: ./pages/intro-splunk.md
hide: false
---

---
src: ./pages/before-storage.md
hide: false
---

---
src: ./pages/after-storage.md
hide: false
---

---
layout: end
---

<div class="text-white font-size-10">
Thank you very much!
</div>
---
layout: default
---

# Splunk Architecture

<div class="flex flex-col gap-2 mt-4">

  <!-- Sources -->
  <div class="grid grid-cols-4 gap-2">
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🖥️ <div class="mt-1 font-semibold">App Servers</div>
    </div>
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      ☸️ <div class="mt-1 font-semibold">Kubernetes</div>
    </div>
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🗃️ <div class="mt-1 font-semibold">Databases</div>
    </div>
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🔌 <div class="mt-1 font-semibold">APIs / Services</div>
    </div>
  </div>

  <div class="text-center text-gray-600">↓</div>

  <!-- Ingestion -->
  <div class="grid grid-cols-3 gap-2">
    <div class="bg-blue-950/40 border border-blue-500/20 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🚚 <div class="mt-1 font-semibold text-blue-300">Universal Forwarder</div>
    </div>
    <div class="bg-blue-950/40 border border-blue-500/20 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      📡 <div class="mt-1 font-semibold text-blue-300">HEC</div>
    </div>
    <div class="bg-blue-950/40 border border-blue-500/20 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      📨 <div class="mt-1 font-semibold text-blue-300">Syslog / TCP</div>
    </div>
  </div>

  <div class="text-center text-gray-600">↓</div>

  <!-- Indexer + Storage side by side -->
  <div class="grid grid-cols-2 gap-2">
    <div class="bg-purple-950/40 border border-purple-500/20 rounded-lg px-4 py-3 text-xs text-gray-300">
      ⚙️ <span class="font-semibold text-purple-300">Indexer</span>
      <div class="text-gray-500 mt-1">parsing · field extraction · routing</div>
      <div class="text-yellow-400/80 mt-1">props.conf / transforms.conf</div>
    </div>
    <div class="bg-red-950/40 border border-red-500/20 rounded-lg px-4 py-3 text-xs text-gray-300">
      🗄️ <span class="font-semibold text-red-300">Indexes</span>
      <div class="text-gray-500 mt-1">hot · warm · cold buckets</div>
      <div class="text-red-400 mt-1">ingestion volume = your bill</div>
    </div>
  </div>

  <div class="text-center text-gray-600">↓</div>

  <!-- Consumption -->
  <div class="grid grid-cols-3 gap-2">
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🔎 <div class="mt-1 font-semibold">SPL Searches</div>
    </div>
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      📊 <div class="mt-1 font-semibold">Dashboards</div>
    </div>
    <div class="bg-white/5 border border-white/10 rounded-lg px-3 py-2 text-center text-xs text-gray-300">
      🔔 <div class="mt-1 font-semibold">Alerts</div>
    </div>
  </div>

</div>

---
layout: two-cols
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
layout: two-cols
---

# The User View
## Where insight is found

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    Your interface is <strong class="text-white">SPL</strong>.
    Everything from ad-hoc queries to automated reports runs through it.
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">1. Pick your index</div>
    <div class="text-gray-500 mt-1">Start narrow — target the index most likely to hold the noise</div>
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">2. Aggregate by transaction</div>
    <div class="text-gray-500 mt-1">Group by <code class="text-blue-300">trx_id</code> to find which flows generate the most volume</div>
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">3. Find patterns</div>
    <div class="text-gray-500 mt-1">Use <code class="text-blue-300">| patterns</code> and <code class="text-blue-300">| cluster</code> to group similar messages automatically</div>
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">4. Build a dashboard</div>
    <div class="text-gray-500 mt-1">Operationalize findings so the whole team can monitor volume trends</div>
  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500">| A typical SPL investigation</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug <span class="text-blue-300">earliest</span>=-7d</div>
    <br/>
    <div><span class="text-purple-400">| stats</span> <span class="text-white">count by</span> <span class="text-blue-300">trx_id, log_level</span></div>
    <div class="text-gray-500 pl-2">-- volume per transaction</div>
    <br/>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-count</span></div>
    <div class="text-gray-500 pl-2">-- worst offenders first</div>
    <br/>
    <div><span class="text-purple-400">| head</span> <span class="text-white">20</span></div>
    <div class="text-gray-500 pl-2">-- focus on top 20</div>
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs text-gray-400">
    <div class="text-white font-semibold mb-2">SPL commands we'll use today</div>
    <div class="flex flex-col gap-1">
      <div><code class="text-blue-300">stats count by</code> — aggregate volume</div>
      <div><code class="text-blue-300">| patterns</code> — cluster similar messages</div>
      <div><code class="text-blue-300">| cluster</code> — ML-based grouping</div>
      <div><code class="text-blue-300">| collect</code> — save to a summary index</div>
      <div><code class="text-blue-300">| outputlookup</code> — export for comparison</div>
    </div>
  </div>

</div>

<div class="absolute bottom-10">
  <div  class="text-white">Michele Caci</div>
  <div class="flex m-0 gap-1">
    <a href="https://github.com/mcaci" target="_blank" alt="Michele's GitHub" title="Michele's GitHub"
      class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
      <carbon-logo-github />
    </a>
    <a href="https://www.linkedin.com/in/michele-caci-47770132/" target="_blank" alt="Michele's Linkedin" title="Michele's Linkedin"
      class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
      <carbon-logo-linkedin />
    </a>
  </div>
</div>
<!-- <img src="/images/michelecaciQR.jpeg" class="absolute bottom-5 right-5 text-right" style="width: 20%; height: auto;"/> -->
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
layout: two-cols
---

# Start With the Right Index

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    Before writing a single query, pick your target carefully.
    An app typically writes to <strong class="text-white">multiple indexes</strong>
    depending on event type — scanning all of them is expensive and noisy.
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-blue-300 font-semibold mb-2">Typical index taxonomy</div>
    <div class="flex flex-col gap-2">
      <div class="flex items-start gap-3">
        <span class="text-green-400 shrink-0">audit</span>
        <span class="text-gray-500">Compliance events — keep long, rarely noisy</span>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-blue-400 shrink-0">functional</span>
        <span class="text-gray-500">Business transactions — moderate volume, high value</span>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-yellow-400 shrink-0">monitoring</span>
        <span class="text-gray-500">Health checks, metrics — repetitive, good cut candidate</span>
      </div>
      <div class="flex items-start gap-3">
        <span class="text-red-400 shrink-0">debug</span>
        <span class="text-gray-500">Stack traces, verbose output — <strong class="text-white">start here</strong></span>
      </div>
    </div>
  </div>

  <div class="bg-yellow-950/30 border border-yellow-500/20 rounded-lg px-4 py-3 text-xs text-yellow-200/80">
    💡 Always scope your query with <code class="text-blue-300">earliest=</code> and
    <code class="text-blue-300">latest=</code> — an unbounded search across a large
    index will time out and hurt other users.
  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="text-xs uppercase tracking-widest text-gray-500 mb-1">
    Find your biggest indexes first
  </div>

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500">| List indexes with their current size</div>
    <br/>
    <div><span class="text-purple-400">| rest</span> <span class="text-blue-300">splunk_server=local</span></div>
    <div><span class="text-blue-300">  /services/data/indexes</span></div>
    <br/>
    <div><span class="text-purple-400">| table</span> <span class="text-white">title,</span></div>
    <div><span class="text-white">  currentDBSizeMB,</span></div>
    <div><span class="text-white">  totalEventCount,</span></div>
    <div><span class="text-white">  maxTotalDataSizeMB</span></div>
    <br/>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-currentDBSizeMB</span></div>
  </div>

  <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs text-gray-400">
    <div class="text-white font-semibold mb-1">What to look for</div>
    <div class="flex flex-col gap-1">
      <div>📈 Indexes growing faster than expected</div>
      <div>📦 Size disproportionate to their business value</div>
      <div>🔎 Indexes nobody has searched in weeks</div>
    </div>
  </div>

</div>

---
layout: two-cols
---

# Log Volume by Transaction

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    If your app logs a <code class="text-blue-300">trx_id</code> (and it should),
    you can immediately answer: <strong class="text-white">which transaction types
    are generating the most lines?</strong>
  </div>

  <div class="flex flex-col gap-2 text-xs">

    <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-purple-300 font-semibold">Step 1 — Count lines per transaction</div>
      <div class="text-gray-500 mt-1">
        Group all events by <code class="text-blue-300">trx_id</code> and count.
        This gives you the distribution of log volume across transaction instances.
      </div>
    </div>

    <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-purple-300 font-semibold">Step 2 — Find the outliers</div>
      <div class="text-gray-500 mt-1">
        Some transactions will have 10× the lines of others.
        Those are your first candidates — either they're genuinely complex,
        or they're logging too verbosely.
      </div>
    </div>

    <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-purple-300 font-semibold">Step 3 — Break down by log level</div>
      <div class="text-gray-500 mt-1">
        A transaction with 500 lines — how many are
        <code class="text-red-400">ERROR</code> vs
        <code class="text-yellow-400">WARN</code> vs
        <code class="text-gray-400">DEBUG</code>?
        If 490 are DEBUG, you have your answer.
      </div>
    </div>

  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500">| Step 1: volume per transaction type</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug
    <span class="text-blue-300">earliest</span>=-24h</div>
    <br/>
    <div><span class="text-purple-400">| rex</span> <span class="text-green-300">"trx_id=(?P&lt;trx_type&gt;[A-Z_]+)"</span></div>
    <div class="text-gray-500 pl-2">-- extract the type prefix from trx_id</div>
    <br/>
    <div><span class="text-purple-400">| stats</span></div>
    <div><span class="text-white">  count,</span></div>
    <div><span class="text-white">  avg(count) as avg_lines,</span></div>
    <div><span class="text-white">  max(count) as max_lines</span></div>
    <div><span class="text-white">  by trx_type</span></div>
    <br/>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-count</span></div>
  </div>

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6 mt-1">
    <div class="text-gray-500">| Step 2: break down by log level</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug
    <span class="text-blue-300">earliest</span>=-24h</div>
    <div><span class="text-blue-300">trx_type</span>=BOOKING</div>
    <br/>
    <div><span class="text-purple-400">| stats</span> <span class="text-white">count by log_level</span></div>
    <div><span class="text-purple-400">| eventstats</span> <span class="text-white">sum(count) as total</span></div>
    <div><span class="text-purple-400">| eval</span> <span class="text-white">pct=round(count/total*100,1)</span></div>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-count</span></div>
  </div>

</div>

---
layout: two-cols
---

# Finding Patterns — Exact Matches

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    Start with what you already suspect. Exact match queries are fast,
    precise, and easy to act on — if a specific message dominates,
    you know exactly what to tell the team to fix.
  </div>

  <div class="flex flex-col gap-2 text-xs">

    <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-blue-300 font-semibold">Top N messages by frequency</div>
      <div class="text-gray-500 mt-1">
        Extract the log message text, strip dynamic values
        (IDs, timestamps, counts), then count by the
        normalized template. The top results are your biggest wins.
      </div>
    </div>

    <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-blue-300 font-semibold">The <code class="text-purple-300">| top</code> command</div>
      <div class="text-gray-500 mt-1">
        Quickest way to find dominant values in any field.
        Combine with <code class="text-blue-300">useother=t</code>
        to see what percentage the top N covers.
      </div>
    </div>

    <div class="bg-yellow-950/30 border border-yellow-500/20 rounded-lg px-4 py-3 text-yellow-200/80">
      💡 A single repeated message accounting for 30%+ of your index
      volume is not unusual — and is almost always safe to suppress
      or downsample.
    </div>

  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500">| Top repeated messages</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug
    <span class="text-blue-300">earliest</span>=-7d</div>
    <br/>
    <div><span class="text-purple-400">| rex</span> <span class="text-green-300">field=_raw</span></div>
    <div><span class="text-green-300">  "(?i)(?:INFO|DEBUG|WARN|ERROR)\s+(?P&lt;msg&gt;.+)"</span></div>
    <br/>
    <div><span class="text-purple-400">| rex</span> <span class="text-green-300">field=msg</span></div>
    <div><span class="text-green-300">  mode=sed "s/[0-9a-f-]{8,}/&lt;ID&gt;/g"</span></div>
    <div class="text-gray-500 pl-2">-- normalize dynamic values</div>
    <br/>
    <div><span class="text-purple-400">| top</span> <span class="text-white">limit=20 msg</span></div>
    <div><span class="text-white">  useother=t showperc=t</span></div>
  </div>

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6 mt-1">
    <div class="text-gray-500">| Quick frequency check on a known message</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug</div>
    <div><span class="text-green-300">"Connection acquired"</span></div>
    <br/>
    <div><span class="text-purple-400">| timechart</span></div>
    <div><span class="text-white">  span=1h count</span></div>
    <div class="text-gray-500 pl-2">-- is it constant or spiky?</div>
  </div>

</div>

---
layout: two-cols
---

# Finding Patterns — Let Splunk Do It

<div class="flex flex-col gap-3 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    When you don't know what to look for,
    <code class="text-blue-300">| cluster</code> group similar
    log lines <strong class="text-white">automatically</strong> —
    no regex needed.
  </div>

  <div class="flex flex-col gap-2 text-xs">

    <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-blue-300 font-semibold">
        <code>| cluster</code>
      </div>
      <div class="text-gray-500 mt-1">
        ML-based clustering — groups events by semantic similarity,
        not just structure. Slower, but surfaces groupings
        that <code class="text-blue-300">| patterns</code> misses.
        Use <code class="text-blue-300">t=0.8</code> to tune sensitivity.
      </div>
    </div>

    <div class="bg-white/5 border border-white/10 rounded-lg px-4 py-3">
      <div class="text-blue-300 font-semibold">
        <code>| collect</code>
      </div>
      <div class="text-gray-500 mt-1">
        Save pattern results to a summary index for trending over time.
        Lets you answer: is this pattern growing week over week?
      </div>
    </div>

  </div>

</div>

::right::

<div class="flex flex-col gap-3 mt-14">

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6">
    <div class="text-gray-500">| Pattern detection — first pass</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug
    <span class="text-blue-300">earliest</span>=-24h</div>
    <br/>
    <div><span class="text-purple-400">| patterns</span></div>
    <div><span class="text-white">  field=_raw</span></div>
    <div><span class="text-white">  maxPatterns=20</span></div>
    <br/>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-count</span></div>
    <div><span class="text-purple-400">| table</span> <span class="text-white">count, pattern</span></div>
  </div>

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6 mt-1">
    <div class="text-gray-500">| ML clustering — deeper grouping</div>
    <br/>
    <div><span class="text-blue-300">index</span>=app_debug
    <span class="text-blue-300">earliest</span>=-24h</div>
    <br/>
    <div><span class="text-purple-400">| cluster</span></div>
    <div><span class="text-white">  field=_raw t=0.8</span></div>
    <br/>
    <div><span class="text-purple-400">| stats</span></div>
    <div><span class="text-white">  count, values(_raw) as sample</span></div>
    <div><span class="text-white">  by cluster_label</span></div>
    <br/>
    <div><span class="text-purple-400">| sort</span> <span class="text-white">-count</span></div>
  </div>

  <div class="font-mono text-xs bg-black/40 border border-white/10 rounded-lg p-4 leading-6 mt-1">
    <div class="text-gray-500">| Save patterns to summary index</div>
    <br/>
    <div><span class="text-purple-400">| collect</span></div>
    <div><span class="text-white">  index=summary_log_patterns</span></div>
    <div><span class="text-white">  addtime=true</span></div>
  </div>

</div>

---
layout: default
---

# Put It All in a Dashboard

<div class="grid grid-cols-3 gap-4 mt-6">

  <!-- Panel 1 -->
  <div class="bg-white/5 border border-white/10 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-gray-500">Panel 1</div>
    <div class="text-white font-semibold text-sm">Index Volume Over Time</div>
    <div class="text-gray-500 text-xs leading-relaxed">
      Timechart of ingestion volume per index.
      Spot regressions — a deployment that suddenly
      doubles log output shows up immediately.
    </div>
    <div class="mt-auto font-mono text-xs text-blue-300/70 pt-2 border-t border-white/10">
      | timechart span=1d sum(kb) by index
    </div>
  </div>

  <!-- Panel 2 -->
  <div class="bg-white/5 border border-white/10 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-gray-500">Panel 2</div>
    <div class="text-white font-semibold text-sm">Top Transaction Types</div>
    <div class="text-gray-500 text-xs leading-relaxed">
      Bar chart of log count by transaction type.
      Lets the team see at a glance which flows
      dominate — updated daily.
    </div>
    <div class="mt-auto font-mono text-xs text-blue-300/70 pt-2 border-t border-white/10">
      | stats count by trx_type | sort -count
    </div>
  </div>

  <!-- Panel 3 -->
  <div class="bg-white/5 border border-white/10 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-gray-500">Panel 3</div>
    <div class="text-white font-semibold text-sm">Log Level Breakdown</div>
    <div class="text-gray-500 text-xs leading-relaxed">
      Pie or stacked bar of ERROR / WARN / INFO / DEBUG ratio.
      A healthy app should have very little DEBUG in production.
    </div>
    <div class="mt-auto font-mono text-xs text-blue-300/70 pt-2 border-t border-white/10">
      | stats count by log_level
    </div>
  </div>

  <!-- Panel 4 -->
  <div class="bg-white/5 border border-white/10 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-gray-500">Panel 4</div>
    <div class="text-white font-semibold text-sm">Top Repeated Patterns</div>
    <div class="text-gray-500 text-xs leading-relaxed">
      Table of the 20 most frequent log templates.
      Each row is a candidate for suppression,
      sampling, or a log level change.
    </div>
    <div class="mt-auto font-mono text-xs text-blue-300/70 pt-2 border-t border-white/10">
      | cluster field=_raw | sort -count
    </div>
  </div>

  <!-- Panel 5 -->
  <div class="bg-white/5 border border-white/10 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-gray-500">Panel 5</div>
    <div class="text-white font-semibold text-sm">Outlier Transactions</div>
    <div class="text-gray-500 text-xs leading-relaxed">
      Transactions with line counts more than 2σ
      above average. These are your debugging sessions
      that forgot to turn off verbose logging.
    </div>
    <div class="mt-auto font-mono text-xs text-blue-300/70 pt-2 border-t border-white/10">
      | eventstats avg(count) as avg, stdev(count) as sd by trx_type
    </div>
  </div>

  <!-- Panel 6 -->
  <div v-click class="bg-blue-950/40 border border-blue-500/30 rounded-xl p-4 flex flex-col gap-2">
    <div class="text-xs uppercase tracking-widest text-blue-400">The goal</div>
    <div class="text-white font-semibold text-sm">A shared, living artifact</div>
    <div class="text-gray-400 text-xs leading-relaxed">
      Save this dashboard and share it with your team.
      Schedule the heavy queries as saved searches
      running nightly — the dashboard becomes a
      <strong class="text-white">cost accountability tool</strong>,
      not a one-off investigation.
    </div>
  </div>

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

<div class="text-white font-size-10">
Thank you very much!
</div>

<div class="absolute bottom-10">
  <div  class="text-white">Michele Caci</div>
  <div class="flex m-0 gap-1">
    <a href="https://github.com/mcaci" target="_blank" alt="Michele's GitHub" title="Michele's GitHub"
      class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
      <carbon-logo-github />
    </a>
    <a href="https://www.linkedin.com/in/michele-caci-47770132/" target="_blank" alt="Michele's Linkedin" title="Michele's Linkedin"
      class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
      <carbon-logo-linkedin />
    </a>
  </div>
</div>
<!-- <img src="/images/octs500.gif" class="absolute bottom-5 right-5 text-right" style="width: 20%; height: auto;"/> -->

