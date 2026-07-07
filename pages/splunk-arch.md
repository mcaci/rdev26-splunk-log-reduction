---
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

# In summary
## Where is the insight found?

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
      <div><code class="text-blue-300">| cluster</code> — ML-based grouping</div>
      <div><code class="text-blue-300">| collect</code> — save to a summary index</div>
      <div><code class="text-blue-300">| outputlookup</code> — export for comparison</div>
    </div>
  </div>

</div>