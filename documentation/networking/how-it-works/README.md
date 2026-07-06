---
icon: head-side-gear
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# How it works

How does Tween+ make TweenService and other alternatives look like relics of the past?

It’s not magic, but it’s a huge shift in how the Client-Server bridge is handled. Instead of overwhelming the network with constant updates, we utilize packed payloads and distributed interpolation, ensuring smooth results for all players and a completely unburdened server. That means the clients do all the hard work of operating the tween in sync locally, rather than relying on immense data sent from a server trying to keep up at a locked 60 fps.

Additionally, there are a plethora of lower-level network optimizations that take the performance even further!

Let’s break down the engineering that makes this possible!

