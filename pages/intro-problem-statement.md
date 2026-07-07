---
layout: statement
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



<div v-click class="mt-6 flex items-center justify-center gap-6 bg-red-950/30 border border-red-500/20 rounded-xl px-6 py-3 mx-auto w-fit">
  <div class="text-center text-4xl font-black text-red-400 shrink-0">We pay for those logs</div>
</div>

<!-- 
So let's see how we can reduce those costs, but first 
-->



---
layout: center
class: text-center
---

# The Problem With Logging Everything

<div class="grid grid-cols-3 gap-5 mt-8">

  <!-- Card 1: Cost -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">💸</div>
    <div class="text-2xl font-bold text-red-400">High Cost</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      Splunk licenses on <strong class="text-white">ingestion volume</strong>.
      <br/>
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-red-400/80 text-xs font-mono">
      Every DEBUG line, every heartbeat, every
      redundant stack trace... <br/> You pay for all of it.
    </div>
  </div>

  <!-- Card 2: Low signal -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">📉</div>
    <div class="text-2xl font-bold text-yellow-400">Low Signal</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      When everything is logged, <strong class="text-white">nothing stands out</strong>.
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-yellow-400/80 text-xs">
      Finding a real error means scanning through
      thousands of irrelevant lines.
    </div>
  </div>

  <!-- Card 3: Unread -->
  <div v-click class="flex flex-col gap-3 bg-white/5 border border-white/10 rounded-2xl p-6">
    <div class="text-3xl">👻</div>
    <div class="text-2xl font-bold text-purple-400">Rarely Read</div>
    <div class="text-gray-300 text-sm leading-relaxed">
      Logs are mostly read when something goes wrong, they <strong>usually stay ignored</strong> otherwise.
    </div>
    <div class="mt-auto pt-3 border-t border-white/10 text-purple-400/80 text-xs">
      You're storing and indexing data that will rarely be read.
    </div>
  </div>

</div>

---
layout: center
class: text-center
---

<div class="flex flex-col items-center gap-8">

  <div class="text-gray-400 text-lg tracking-widest uppercase">The good news</div>

  <h1 v-click class="text-5xl font-black leading-tight max-w-2xl">
    You don't need to log less.<br/>
    <span class="text-blue-400">You need to log smarter.</span>
  </h1>

  <div v-click class="mt-6 flex items-center justify-center gap-6 bg-white/5 border border-white/10 rounded-xl px-6 py-3 mx-auto w-fit">
    <div class="text-center text-lg font-medium text-gray-300">
      And <strong class="text-white">pattern detection with Splunk</strong> is only one of the available tools from the box
    </div>
  </div>

</div>

<!-- 
If your team uses Splunk, you already have some tools available to
- Find the noise in the logs
- Detect frequent patterns
- Make data-driven decisions about what to cut

-->