---
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

# Compress the data

To optimize further, we have to understand how data is represented in computers.

Bits are essentially switches inside our computers used to represent all data. A boolean, for example, can be stored with just a single bit. For more complex data, we use more bits, where each combination of bits represents a different value. Typically, computers allocate memory in groups of 8 bits, called bytes. Because of this, a standalone boolean often ends up occupying a full 8-bit byte of memory. However, when sending data over the network, custom bit-packing allows us to squeeze multiple pieces of data into those bits to maximize efficiency.​

If we look closer at standard network payloads, we often find unnecessary bits. By default in Luau, numbers are stored as `Float64`s – meaning they contain decimals, use 64 bits of data, and are signed (they can store negative values). The issue is that we don't need negative values, decimals, or 64 whole bits just to store a number like 1.

A `Float64` includes a massive range of numbers from $$-1.79 \times 10^{308}$$ all the way up to $$1.79 \times 10^{308}$$, with up to 16 digits of decimal precision. In many cases, that’s extreme overkill. Therefore, we can drastically lower the amount of bits used to represent our data.

One example of this optimization in Tween+ is our server tween identifiers. They are intentionally limited to a range of 0–255 because that fits perfectly into a `UInt8` (an unsigned 8-bit integer). While this caps the maximum number of active server tweens at 256 at any given moment, it significantly reduces network traffic. Keep in mind that this type of compression isn't limited to numbers—it can be applied to all sorts of data!

But there is specific data that is often completely overlooked.

Every time we send something over the network, we have to send an identifier for the remote event so the client knows which event is being triggered – for example, informing the client that we're firing a `StartTween` event. This is exactly what happens behind the scenes when you fire a `RemoteEvent` in Roblox. Because of this overhead, it’s best to minimize the total number of network fires.

Instead of firing separate remotes, we can funnel everything through a single event. By batching all of our data together, we can fire that single event no more than once per frame. This allows us to pack and structure our own internal event identifiers however we want, drastically cutting down on packet overhead when multiple events trigger in the same frame.

Those are the main principles behind the networking library Packet (and its fork, Packet+), which Tween+ relies on for its global tweens feature – even if it gets far more complex behind the scenes.
