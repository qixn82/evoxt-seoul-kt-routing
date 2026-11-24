# Evoxt Seoul: Real Korean IP or Relabeled Japan? KT Routing, Measured Latency, All 11 Plans, and Who Should Skip the $2.99 Tier

Evoxt's Seoul location starts at **$2.99/month** for 1 vCPU, 512MB RAM and 5GB of NVMe, sits in a KINX facility with Korea Telecom as primary transit, and returns roughly **1ms** to points inside Seoul. The problem with the $2.99 plan is the disk, not the RAM: 5GB disappears after the OS and a handful of packages, so most readers should start at VM-1 for **$5.99** with 2GB RAM and 20GB of storage.

What follows covers the parts that decide whether this node fits your use case: whether the server is genuinely in Korea, what the hardware actually benchmarks at (the 6.0 GHz figure on Evoxt's homepage is a manufacturer ceiling, not what you will see in `lscpu`), how China Telecom, Unicom and Mobile each reach it, which streaming services pass and which do not, the full eleven-plan price list, and the workloads where you should not buy this at all.

## Quick pick before you read the rest

> **Most readers:** VM-1 at $5.99/month (1 vCPU, 2GB RAM, 20GB NVMe, 1TB transfer). It is the first plan with enough disk for a real OS install plus updates.
> **Single-user proxy, DNS resolver, uptime monitor, tiny bot:** VM-0.5 at $2.99 is enough, and the 5GB limit stops mattering.
> **Game server or anything with a map/world file:** VM-4 at $23.99 (4 vCPU, 8GB RAM, 60GB).
> **Skip Evoxt Seoul entirely if:** you need Windows with GUI, a multi-container Docker stack, or a production database with growth headroom under 30GB.

👉 [Start with VM-1 on the Seoul node](https://bit.ly/EvoXt)

## All 11 Seoul plans, standard-tier pricing

Seoul sits in Evoxt's **standard** price tier, alongside the US, UK, Germany, Tokyo and Malaysia. You are not paying the Hong Kong or Osaka premium here, and every plan below carries a 1Gbps port, one IPv4, IPv6, and a free weekly offsite backup.

| Plan | vCPU / RAM | NVMe SSD | Monthly transfer | Price (monthly) | Order |
| --- | --- | --- | --- | --- | --- |
| VM-0.5 | 1 core / 512MB | 5GB | 500GB | $2.99 | [Deploy VM-0.5](https://bit.ly/EvoXt) |
| VM-0.75 | 1 core / 1GB | 10GB | 750GB | $4.99 | [Deploy VM-0.75](https://bit.ly/EvoXt) |
| VM-1 | 1 core / 2GB | 20GB | 1TB | $5.99 | [Deploy VM-1](https://bit.ly/EvoXt) |
| VM-1.5 | 2 cores / 2GB | 20GB | 1.5TB | $6.95 | [Deploy VM-1.5](https://bit.ly/EvoXt) |
| VM-2 | 2 cores / 4GB | 30GB | 2TB | $11.99 | [Deploy VM-2](https://bit.ly/EvoXt) |
| VM-3 | 4 cores / 4GB | 30GB | 3TB | $14.99 | [Deploy VM-3](https://bit.ly/EvoXt) |
| VM-4 | 4 cores / 8GB | 60GB | 4TB | $23.99 | [Deploy VM-4](https://bit.ly/EvoXt) |
| VM-6 | 8 cores / 8GB | 60GB | 5TB | $29.99 | [Deploy VM-6](https://bit.ly/EvoXt) |
| VM-8 | 8 cores / 16GB | 80GB | 6TB | $47.99 | [Deploy VM-8](https://bit.ly/EvoXt) |
| VM-12 | 16 cores / 16GB | 80GB | 8TB | $60.95 | [Deploy VM-12](https://bit.ly/EvoXt) |
| VM-16 | 16 cores / 32GB | 100GB | 10TB | $95.99 | [Deploy VM-16](https://bit.ly/EvoXt) |

A note on the VM-0.75 and VM-1.5 oddballs: both exist to solve one specific shortage. VM-0.75 gives you double the RAM and disk of the entry plan without touching the CPU count. VM-1.5 doubles the cores for $0.96 more than VM-1, which makes it the better buy if your workload is concurrent rather than single-threaded. For a proxy, a game server or an API relay, single-thread speed matters more than a second core.

## Is this actually a Seoul server?

Yes. Evoxt runs its own autonomous system, **AS149440**, and the Seoul deployment connects to **KINX** with primary transit through **Korea Telecom**. That matters because the cheap "Korea" VPS market has a well-known habit of selling a Tokyo or Osaka box with a Korean-sounding product name, which you discover when every traceroute lands in Japan and the geo-database says otherwise.

The practical test after deployment is a geo-lookup on the assigned IP. Independent testing of the Seoul range returned a KT native broadcast address with a Scamalytics risk score of **0**, and Korean-region unlocks behaved the way a domestic Korean IP should. Evoxt itself has expanded to 16 regions as of Q3 2025, with Seoul among the more recent additions, so the node is part of a global build-out rather than a one-off reseller arrangement.

## Hardware: the 6.0 GHz claim versus what you get

Evoxt's homepage compares its CPUs against AWS (2.4 GHz), Azure (2.3 GHz) and Google Cloud (2.2 GHz) at 6.0 GHz, and the same page notes the data was collected in 2020. Treat 6.0 GHz as the silicon's rated turbo ceiling on AMD EPYC 9004 (Genoa) parts, not as a per-VM guarantee.

Independent benchmarking of the entry plan measured a single-core boost of **4291 MHz**, with these results:

- **Geekbench 5:** 2080 single-core, 2335 multi-core
- **SysBench CPU:** 6965 single-core
- **Memory:** read above 100 GB/s, write around 40 GB/s

The multi-core number looks low next to the single-core one because the test machine had one vCPU. That is the whole point of this product line: the clock speed carries single-threaded work. Proxies, game servers, API bridges, scrapers and small web apps all live or die on single-thread latency, which is why the plan list tops out at 16 cores rather than pushing core count as the headline.

Evoxt offers no CPU burst surcharge and states plainly that a $2.99 order costs $2.99. Third-party benchmarking outfit VPSBenchmarks placed Evoxt second in its 2025 list of best VPS providers under $25, noted a top-three position since 2022, and its February 2026 round confirmed that single-core performance matched the marketing. The company was founded in 2020 and is Malaysian, which is worth knowing if a long operating history is a hard requirement for you.

## Latency and routing: three Chinese carriers, three different paths

Local Seoul latency comes in around **1ms**. From mainland China, the same test plan showed:

| Route | Measured latency |
| --- | --- |
| China Telecom | ~77ms |
| China Unicom | ~128ms |
| China Mobile | ~115ms |
| Shanghai Telecom | as low as 25ms |

Outbound, Telecom runs from Beijing/Inner Mongolia onto the 163 backbone (AS4134) and hands off directly to KT in Seoul. Unicom aggregates through Shanghai/Beijing on AS4837 and also connects straight to KT. Mobile takes a different path: Shanghai backbone, then Japan via NTT/CMI, then KT.

Return traffic is cleaner than the outbound picture suggests. Telecom returns KT to China Telecom 163 directly, Unicom returns on AS4837, and Mobile comes back through China Mobile's Hong Kong CMI before rejoining the mainland backbone.

So the honest summary of the network: **Telecom and Unicom get a direct KT path in both directions; Mobile does not.** The outbound detour through Japan is the one soft spot, and it is the thing to watch during peak hours if you are on a southern China Mobile connection. Everything else about the path is what people pay premium money for elsewhere.

👉 [Check current Seoul availability and pricing](https://bit.ly/EvoXt)

## IP quality and streaming: what passes, what doesn't

This is the section most write-ups blur, so here is the specific result set from the tested Seoul range:

- **Netflix:** works, Korean region library
- **TikTok:** works, Korean region
- **YouTube:** works, Korean region
- **ChatGPT:** works
- **Disney+:** may be blocked on this IP range
- **Reddit:** may be blocked on this IP range

If your reason for wanting a Korean IP is Korean Netflix, TikTok moderation work, or a Korean-region view of YouTube, the node does the job. If you are buying specifically for Disney+ or for Reddit access, test the assigned IP first and be ready to swap it, because IP reputation varies by block and Evoxt lets you manage and reassign IPs from the panel.

The KT native broadcast status is the underlying reason the Korean unlocks hold up. Data-center IPs resold from unrelated ranges tend to fail streaming checks within weeks; a carrier-broadcast Korean address on KT's own network behaves like a Korean residential-adjacent connection as far as the geo-databases are concerned.

## Real cost: metered traffic, cheap overage, no surprises

Two corrections to what circulates on coupon and review sites. First, **traffic is metered, not unmetered**: each plan includes a monthly allowance and bills overage after that. Second, the Seoul allocations start at **500GB** on VM-0.5, not the 250GB figure some tables show, which appears to be copied from the premium regions where allowances are roughly halved for the same money.

Overage on the standard tier, which includes Seoul, runs **$3 per TB**. For comparison, premium regions bill $12/TB and Premium Plus bills $24/TB. At $3/TB, blowing through your allowance is an inconvenience rather than a budget event.

Add-on pricing, for when you outgrow a plan without wanting to migrate:

- Extra IPv4: **$3/month**
- Extra vCPU: **$3/core/month**
- Extra RAM: **$2/GB/month**

Billing is monthly by default, and Evoxt allows prepayment for up to three years to lock the rate. There is no separate charge for private networking between your own VMs, which makes a two-box setup (one public-facing, one internal) cheaper than it looks.

## Who should not buy this

**Anyone who needs Windows with a desktop.** The OS list includes Windows, but a 512MB or 1GB instance will not run it usefully, and once you are at VM-2 or above for a Windows license plus RAM, you are in a different budget bracket. Linux is the assumption here.

**Multi-container Docker stacks.** A reverse proxy, an app, a database and a Redis instance on one box will eat 2GB before the application does anything. Start at VM-2 or VM-3.

**Databases with growth.** The 5GB and 10GB disks are the real constraint, not the RAM. A MySQL or Postgres instance plus its WAL and backups will fill 20GB faster than you expect. VM-4 with 60GB is the sane floor for anything storing real data.

**Anyone who needs a polished control panel.** Evoxt's panel is functional rather than refined: firewall, VNC, monitoring, IP management, cloning, sub-accounts, rescue mode, all present, nothing decorative. If you want a hosting panel bundled in, you install one yourself.

**Anyone who wants a decade of track record.** The company is six years old. The 99.99% uptime guarantee and user reports suggest competent operations, but it is not a legacy provider and should not be treated as one for a business-critical single point of failure.

## Ordering, and the two mistakes people make

The deployment flow is short: create an account with name, email and password (Evoxt states it typically requires nothing beyond a name to identify you, and there is no document upload), add credit or set up a subscription by card, PayPal, or crypto (Bitcoin, Litecoin, Ethereum), pick your region, pick a plan, pick an OS from Ubuntu, Debian, CentOS, Windows, FreeBSD or the one-click templates, and wait about **2.5 minutes** for the IP and root credentials.

**Mistake one: leaving the region dropdown on Tokyo.** Evoxt sells both a Tokyo and a Seoul location, and the default selection is not guaranteed to be Korea. Confirm the location field reads Seoul before you pay.

**Mistake two: assuming a coupon site's code will work.** Third-party coupon aggregators list Evoxt discounts with a well-documented habit of being expired or region-restricted. Evoxt's own guidance is to go by what the console shows at checkout. If a code applies, you will see the reduced total before you confirm; if it doesn't, don't chase it.

After the box is up, two minutes of work pays for itself: run a geo-lookup to confirm the IP reads as Korean, and set firewall rules in the panel rather than planning to do it later.

👉 [Deploy a Seoul VPS and verify the IP yourself](https://bit.ly/EvoXt)

## FAQ

**Does Evoxt Seoul give me a Korean IP for streaming?**
Yes. The range tested as KT native broadcast with a Scamalytics score of 0, and Netflix, TikTok and YouTube all resolved to Korean regions. Disney+ and Reddit may be blocked on the same range.

**Is $2.99 enough to host a website?**
For a static site, a small proxy, or a single lightweight service, yes. For WordPress with plugins, no: 5GB of disk and 512MB of RAM will be tight from the first update cycle, and you should be on VM-1 at minimum.

**How fast is it from mainland China?**
Telecom around 77ms with Shanghai as low as 25ms, Unicom around 128ms, Mobile around 115ms. Telecom and Unicom connect directly to KT in both directions; Mobile's outbound path detours through Japan and returns via Hong Kong CMI, so peak-hour variance is most likely there.

**What happens if I exceed the monthly transfer?**
Standard-tier overage, which covers Seoul, bills at $3/TB. Traffic is metered, so budget for it if you are pushing large volumes.

**Can I upgrade later?**
Yes. Plans scale from the control panel, and add-ons (extra IP at $3/month, extra vCPU at $3/core/month, extra RAM at $2/GB/month) let you resize without migrating.

## Which plan to actually order

Match the plan to the smallest resource that will not become the bottleneck in three months, and remember that on this product line the bottleneck is almost always disk.

- **One user, one tunnel, or a DNS/monitoring box:** VM-0.5, $2.99. Nothing above it buys you anything you will notice.
- **A small site, a bot with a database, a single Docker container:** VM-1, $5.99. The first plan where 20GB stops being a daily worry.
- **Concurrent connections rather than raw speed:** VM-1.5, $6.95. Two cores for under a dollar more than VM-1.
- **Game server, media relay, anything with a world file:** VM-4, $23.99.
- **Several services on one box:** VM-6 at $29.99 or VM-8 at $47.99, depending on whether RAM or disk runs out first.

Before you pay, confirm the region field says Seoul, and check what the cart total actually shows after any code you try. Those two checks are worth more than any review, including this one.

👉 [Open the Evoxt console and select Seoul](https://bit.ly/EvoXt)
