---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
base: /rdev26-splunk-log-reduction/
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

# Splunk provides tools to show you places where you can cut logs

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