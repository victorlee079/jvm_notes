# My Learning Notes on JVM

***GC Mechanism***
==================

**Marking Objects**
-------------------

Tracing Garbage Collection was adopted in Java.
(Another way is to do reference counting which is memory consuming and requires additional handling to deal with circular reference)

Start from GC Root and scan from top to bottom. If an object cannot be referenced, it will be marked.

GC Root
- Class
- Local Varirable
- Static 
- JNI
- JMXBean
- Objects locked by synchronized
- Constant Pool

How to dump
1. jps/jmap
2. JVisualVM

**Recycle**
-----------

a. Mark-Sweep
Steps:
- Stop the world
- Scan from root and Mark all referred objects in their headers
- Full scan and clean objects without marker

Not efficient, STW, fragment

b. Copy
Steps:
- Divide memory into 2 regions (A and B)
- Copy live objects to B
- Clean A
- Swap the role of two regions

Simple, fast, no mark and delete, no fragment
Memory doubled, good if # live object is small

c. Mark-Compact
Steps:
1. Scan from root and Mark all referred objects in their headers
2. Compact live objects to one side sequentially
3. Clean other

No fragment, STW

Speed: Copy > Mark-Sweep > Mark-Compact
Space: Mark-Sweep = Mark-Compact < Copy
Movement: Copy, Mark-Compact

**Generational Collecting**

Young Gen
- Short life cycle, Frequent Recycle, Smaller region
- (b) Copy

Tenured Gen
- Larger region, Longer Life Cycle
- (c) Mark-Compact

Example: CMS
