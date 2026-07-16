## Hardware

### HP ELiteDesk 800 G5 Mini
**Specs:** Intel Core i7-9700T | 512GB NVMe (OS) | 2TB Samsung NVMe (VM/LXC storage) | 24GB RAM (single 16GB factory + 8GB SODIMM upgrade)

**Why this system:**
Initially I was looking into building a small form factor PC to become my server rig. After researching many different components (ITX vs M-ATX, Intel vs AMD), PC cases, cooling solutions (liquid vs air), and upfront/ongoing cost, I determined an Intel-based mini PC would best fit my needs based off of the low power draw (due to the "T" variant CPU), low heat generation, and the small footprint. While the extra cores from a Ryzen 9 series CPU would have provided significant overhead, the TDP draw of 105w (minimum) 24/7 is not cost effective. Given that, I ultimately made the decision to go with an enterprise-grade Intel mini PC with reliability, low upfront/ongoing cost in mind. 

**Upgrades:**
From a previous system I was able to source a spare 8GB SODIMM RAM which I added for future scalability when running multiple VMs or LXCs. Additionally, I purchased another storage drive to separate the OS and data if any failure occurs. A full storage drive could impact OS functionality and consistent stress from RWD onto the used 512 GB NVMe (where the OS lives) could unexpectedly kill the drive. The 2TB Samsung NVMe handles all VM disks, LXC containers, and backups.

**System Health Validation:**
