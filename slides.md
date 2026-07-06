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
hide: true
---

---
src: ./pages/problem-statement-intro.md
hide: false
---

---
src: ./pages/whoami.md
hide: false
---

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
src: ./pages/splunk-arch.md
hide: true
---

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

---
layout: end
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

