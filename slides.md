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
src: ./pages/whoami.md
hide: false
---

---
src: ./pages/intro-talkoutline.md
hide: true
---

---
src: ./pages/intro-problem-statement.md
hide: false
---

---
src: ./pages/intro-splunk.md
hide: false
---

---
src: ./pages/before-indexing.md
hide: false
---

---
src: ./pages/after-indexing.md
hide: false
---

---
src: ./pages/splunk-arch.md
hide: true
---

---
---

# In summary
## What have we seen today?

<div class="flex flex-col gap-4 mt-4 pr-4">

  <div class="text-gray-400 text-sm leading-relaxed">
    Where are the <strong class="text-white">costs</strong> in logging and how you can use Splunk to help you ideintify and reduce them.
  </div>

  <br/>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">1. Act at indexing time</div>
    <div class="text-gray-500 mt-1">Use structured logging and <code>props.conf</code> and <code>transforms.conf</code> to help splunk detect fields, route events and discard them</div>
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">2. Analyze your logs and find insights on how to reduce them </div>
    <div class="text-gray-500 mt-1">Pick the highest logging index, aggregate by transactions, find patterns</div>
  </div>

  <div v-click class="bg-white/5 border border-white/10 rounded-lg px-4 py-3 text-xs">
    <div class="text-purple-300 font-semibold">3. Build alerts and dashboard</div>
    <div class="text-gray-500 mt-1">Operationalize findings so the whole team can monitor the logging trends</div>
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

