---
# You can also start simply with 'default'
theme: the-unnamed
background: './images/title-background.webp'
# some information about your slides (markdown enabled)
title: Stepwise refactoring java legacy applications
info: |
  ## Stepwise refactoring java legacy applications
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
src: './pages/01-title.md'
layout: cover
---
---
layout: intro
layout: about-me

helloMsg: Hi there!
name: Andreas Haardt
#imageSrc: <image url>
job: Team Lead @STARFACE
line1: +25 years professional experience
line2: T-Shaped :)
social1: Say hi at @linkedin!
social2: 
social3: 
transition: fade-out
---
<!--
Title
-->
---
transition: slide-left
layout: center
src: './pages/03-agenda.md'
---
<!--
Hi there
-->
---
transition: slide-left
src: './pages/04-kubernetes.md'
---
<!--
Overview
-->
---
transition: slide-left
src: './pages/05-manifests.md'
---
<!--
ports and adapters
-->
---
transition: slide-left
src: './pages/06-helm-flux.md'
---
<!--
ports and adapters diagram
-->
---
transition: slide-left
src: './pages/07-platform.md'
---
<!--
Platform
-->
---
src: './pages/08-dapr.md'
---
<!--
structure
-->
---
layout: default
transition: slide-left
src: './pages/09-spring-di.md'
---
<!--
spring di
-->
---
layout: two-cols
transition: slide-left
src: './pages/10-modules.md'
---
<!--
modules
-->
---
layout: two-cols
transition: slide-left
src: './pages/11-modules2.md'
---
<!--
modules2
-->
---
layout: center
transition: slide-left
src: './pages/12-legacy-app.md'
---
<!--
code
-->
---
layout: center
transition: slide-left
src: './pages/13-pusher-improver.md'
background: './images/pusher-improver2.jpeg'

---
<!--
pusher
-->
---
layout: default
transition: slide-left
src: './pages/14-conclusion.md'
background: './images/wrangler-fig.webp'

---
<!--
pusher
-->
---