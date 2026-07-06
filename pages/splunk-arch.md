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