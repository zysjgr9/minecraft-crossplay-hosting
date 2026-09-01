# Minecraft Crossplay Server Hosting: How to Let Java & Bedrock Players Join the Same World — Setup, Plugins, and the Right Plan to Pick

If you've ever tried to get a Minecraft server running for a friend group where half the people are on PC and the other half are on Xbox, PlayStation, Switch, or a phone, you already know the headache. Java Edition and Bedrock Edition don't talk to each other out of the box. Someone always ends up left out. That's the whole reason "minecraft crossplay server hosting" became a search term in the first place — people want one shared world where everyone, on whatever device, can show up and build together.

This is the practical guide I wish I'd had when I was first trying to set this up. We'll walk through what crossplay actually means, why it's not as simple as "just buy any Minecraft server," the plugin stack that makes it work (GeyserMC and Floodgate), how much RAM you really need, and how a host like ExtraVM fits into the picture — including a full breakdown of their Minecraft plans so you can pick the right one without overpaying.

## What "Crossplay" Actually Means in Minecraft

There's a common misconception that you can flip a switch and Bedrock players will magically join a Java server. That's not how it works. Java Edition and Bedrock Edition are two separate games with different network protocols, different block states in some edge cases, and different item handling. A vanilla Java server cannot accept a Bedrock client connection natively. Ever. Mojang has said this plainly.

What we call "crossplay" in the hosting world is really a translation layer. You run a Java Edition server (usually Paper, Spigot, or Purpur — the optimized server software that supports plugins), and you install a plugin called **GeyserMC** that listens on a separate UDP port. When a Bedrock player connects, Geyser translates their Bedrock protocol packets into Java protocol packets in real time, so the Java server thinks it's talking to a Java client. A companion plugin called **Floodgate** lets Bedrock players join without needing a Java/Microsoft account — they authenticate with their Xbox Live or Bedrock account instead.

So when a hosting provider says they "support crossplay," what they really mean is: their Java Minecraft plans let you install Paper/Spigot plugins and open an extra UDP port for Geyser. That's it. Some hosts also sell a separate Bedrock-only server plan, but that's not crossplay — that's just Bedrock Edition hosting, where only Bedrock players can join.

## Why Most "Minecraft Hosts" Don't Actually Do Crossplay Well

Here's the thing that trips people up. A lot of big-name Minecraft hosts advertise "Java and Bedrock support," but if you read the fine print, they mean they offer two separate products — a Java server product and a Bedrock server product — not one server that both editions can join simultaneously. To get true crossplay, you need:

- A Java Edition server running Paper, Spigot, or Purpur (vanilla won't work — it has no plugin API)
- The ability to install GeyserMC and Floodgate as plugins
- An open UDP port (Geyser uses UDP, not TCP) that the host allows you to configure
- Enough RAM to handle the translation overhead on top of the normal server load
- A host that doesn't block UDP or lock down ports

This is where a lot of budget hosts quietly fail. They'll sell you a cheap Java server, but their firewall blocks the UDP port Geyser needs, or their control panel doesn't let you open one, or they only give you a fixed set of ports. Suddenly your "crossplay server" only works for Java players and the Bedrock players get connection errors.

## The Plugin Stack: GeyserMC + Floodgate (and Sometimes ViaVersion)

If you're setting this up, here's the short version of what you install on a Paper/Spigot server:

1. **GeyserMC** — the core translation plugin. Place the `Geyser-Spigot.jar` in your `plugins` folder, restart the server, and it spins up a UDP listener (default port 19132, the standard Bedrock port). Bedrock players connect to your server IP on that port.

2. **Floodgate** — handles Bedrock authentication so players don't need a Java account. Without Floodgate, Bedrock players would need to log in with a Microsoft Java account, which defeats the entire point for console/mobile players. With Floodgate, they use their Xbox Live identity.

3. **ViaVersion** (optional, for multi-version Java support) — if you want Java players on older or newer client versions to all join the same server, ViaVersion + ViaBackward lets you run one server version and accept a range of Java client versions. Not strictly required for crossplay, but common in crossplay servers because your player base is already mixed.

The setup itself is genuinely under five minutes once you have a Paper server running: drop the jars in `plugins/`, restart, edit the `config.yml` files if you want to change the Bedrock port or MOTD, and you're done. The harder part is making sure your host actually lets you do this — which is why host choice matters more than people realize.

## How Much RAM Does a Crossplay Server Really Need?

This is the question everyone asks and most guides get vague about. Let's be specific. Geyser adds overhead per Bedrock player — roughly 256 MB to 512 MB of extra RAM per Bedrock player on top of what the Java server is already using, because Geyser has to maintain a translation state for each connected Bedrock client. So your RAM budget isn't just "vanilla server RAM" — it's "vanilla server RAM + plugin overhead + Geyser overhead per Bedrock player."

A reasonable starting point:

- **2 GB**: A tiny vanilla-ish server with maybe 3-4 Java players and 1-2 Bedrock players. You'll feel the ceiling fast.
- **4 GB**: The realistic minimum for a small crossplay server. Handles ~10 Java players plus a handful of Bedrock players, light plugins, vanilla world. This is where most friend-group servers should start.
- **6 GB**: Comfortable for ~15-20 mixed players with moderate plugins (essentials, economy, land claiming).
- **8 GB**: Good for ~25-30 mixed players or a moderately modded server with crossplay.
- **10-12 GB**: Heavy modpacks (100+ mods) with crossplay. Modpacks eat RAM fast, and adding Geyser on top means you want headroom.
- **16 GB and up**: Large communities, heavy modpacks, or servers expecting 40+ mixed players.

The rule of thumb: whatever RAM you'd pick for a Java-only server, add 1-2 GB if you expect more than a couple Bedrock players regularly. Geyser is not free.

## Where ExtraVM Fits In (and Why It Works for Crossplay)

I'm going to talk about ExtraVM here because it's the host tied to this whole exercise, but also because it happens to do the specific things crossplay needs without fighting you.

ExtraVM has been around since 2014 — they're a registered US company (ExtraVM LLC, Delaware) and they've been doing Minecraft hosting for over a decade, which in this industry is genuinely a long time. The relevant parts for crossplay:

- **Java Edition servers run on Paper/Spigot/Purpur/Forge/Fabric** with one-click modpack installers for CurseForge, Feed The Beast, Modrinth, ATLauncher. So GeyserMC and Floodgate install cleanly — they're just Spigot/Paper plugins.
- **Full SFTP and file manager access** means you can drop plugin jars directly into the `plugins` folder without begging support to do it for you. This matters because some locked-down hosts make you submit a ticket to install anything.
- **Custom game panel with a web console** — you can restart the server, watch the console, edit configs, and open the Geyser UDP port from the browser. No SSH required.
- **DDoS protection included** at US, Europe, and Singapore locations at no extra cost. Crossplay servers that get listed publicly are DDoS magnets, so this isn't a nice-to-have.
- **Locations in the US, Europe (Germany), Singapore, and Australia (Sydney)** — pick the one closest to most of your players. Latency matters more for Bedrock players on mobile connections than for Java players on wired PCs.
- **5-day money-back guarantee** on Minecraft plans, so if you set up Geyser and your Bedrock friends can't connect for some reason, you're not stuck paying for a month.
- **In-house US-based support** — not outsourced. When you open a ticket about a Geyser port issue, you're talking to someone who actually knows the infrastructure.

The one thing to know upfront: ExtraVM's standard Minecraft plans are Java Edition plans. They do offer a separate Bedrock Edition hosting product (different control panel), but if your goal is crossplay — one world, both editions — you want the **Java plan with GeyserMC installed**, not the Bedrock product. The Bedrock product is for Bedrock-only servers.

## ExtraVM Minecraft Plans: Full Breakdown (US & Europe Locations)

ExtraVM prices Minecraft hosting at **$3.00 per GB per month** for US and Europe locations. The smallest 1 GB plan is $3.20/mo (slightly above the per-GB rate, likely a floor price), and from 2 GB up it's a clean $3.00/GB. Singapore and Australia locations are $5.00/GB because those datacenters cost more to operate.

Here's the full plan table for the US/EU locations — these are the ones most crossplay server operators will want, since they're the cheapest and DDoS-protected:

| Plan | RAM | Suggested Players (Mixed Crossplay) | Monthly Price (US/EU) | Get Started |
| --- | --- | --- | --- | --- |
| 1 GB | 1 GB | Not recommended for crossplay (too tight) | $3.20/mo | [Order 1 GB Plan](https://bit.ly/Extravm) |
| 2 GB | 2 GB | ~5-8 mixed players, vanilla only | $6.00/mo | [Order 2 GB Plan](https://bit.ly/Extravm) |
| 3 GB | 3 GB | ~10 mixed players, light plugins | $9.00/mo | [Order 3 GB Plan](https://bit.ly/Extravm) |
| 4 GB | 4 GB | ~15 mixed players, light-moderate plugins | $12.00/mo | [Order 4 GB Plan](https://bit.ly/Extravm) |
| 5 GB | 5 GB | ~18-20 mixed players, moderate plugins | $15.00/mo | [Order 5 GB Plan](https://bit.ly/Extravm) |
| 6 GB | 6 GB | ~20-25 mixed players, moderate plugins | $18.00/mo | [Order 6 GB Plan](https://bit.ly/Extravm) |
| 7 GB | 7 GB | ~25 mixed players, moderate plugins | $21.00/mo | [Order 7 GB Plan](https://bit.ly/Extravm) |
| 8 GB | 8 GB | ~30 mixed players or light modpack + crossplay | $24.00/mo | [Order 8 GB Plan](https://bit.ly/Extravm) |
| 10 GB | 10 GB | ~35 mixed players or medium modpack + crossplay | $30.00/mo | [Order 10 GB Plan](https://bit.ly/Extravm) |
| 12 GB | 12 GB | Heavy modpack (100-200 mods) + crossplay | $36.00/mo | [Order 12 GB Plan](https://bit.ly/Extravm) |
| 15 GB | 15 GB | Large community, heavy modpack + crossplay | $45.00/mo | [Order 15 GB Plan](https://bit.ly/Extravm) |
| 20 GB | 20 GB | 200+ mod packs, large crossplay community | $60.00/mo | [Order 20 GB Plan](https://bit.ly/Extravm) |

> **Note on per-plan links:** ExtraVM's public Affiliate Product IDs list only covers VPS and Web Hosting products — the Minecraft plan product IDs aren't published, so the order links above route through the main affiliate portal where you can select your exact RAM and location. If you want a specific plan, just pick the RAM slider on the Minecraft page after clicking through.

### Singapore & Australia Locations

If most of your players are in Asia or Oceania, the Singapore and Sydney locations are worth the premium. Pricing is **$5.00/GB**:

| RAM | Monthly Price (SG/AU) | Get Started |
| --- | --- | --- |
| 1 GB | $5.00/mo | [Order SG/AU 1 GB](https://bit.ly/Extravm) |
| 2 GB | $10.00/mo | [Order SG/AU 2 GB](https://bit.ly/Extravm) |
| 3 GB | $15.00/mo | [Order SG/AU 3 GB](https://bit.ly/Extravm) |
| 4 GB | $20.00/mo | [Order SG/AU 4 GB](https://bit.ly/Extravm) |
| 5 GB | $25.00/mo | [Order SG/AU 5 GB](https://bit.ly/Extravm) |
| 6 GB | $30.00/mo | [Order SG/AU 6 GB](https://bit.ly/Extravm) |
| 7 GB | $35.00/mo | [Order SG/AU 7 GB](https://bit.ly/Extravm) |
| 8 GB | $40.00/mo | [Order SG/AU 8 GB](https://bit.ly/Extravm) |
| 10 GB | $50.00/mo | [Order SG/AU 10 GB](https://bit.ly/Extravm) |
| 12 GB | $60.00/mo | [Order SG/AU 12 GB](https://bit.ly/Extravm) |
| 15 GB | $75.00/mo | [Order SG/AU 15 GB](https://bit.ly/Extravm) |

The Australian location includes basic local DDoS filtering rather than the full enterprise protection at US/EU/SG, which is worth knowing if you expect to be a target.

## Step-by-Step: Setting Up a Crossplay Server on ExtraVM

Here's the actual process from zero to "my Bedrock friend just joined my Java world."

1. **Pick your plan and location.** For a first crossplay server with a friend group, the 4 GB US/EU plan at $12/mo is the sweet spot — enough headroom for Geyser overhead and a few plugins. 👉 [Start with the 4 GB plan here](https://bit.ly/Extravm).

2. **Deploy and choose server software.** After payment, the server deploys instantly. In the ExtraVM game panel, pick **PaperMC** as your server type (not Vanilla — you need the plugin API). Pick the latest stable Minecraft version your Java players are on.

3. **Install GeyserMC.** In the panel's plugin/modpack installer, search for Geyser, or upload `Geyser-Spigot.jar` via the file manager or SFTP into `/plugins/`. Restart the server.

4. **Install Floodgate.** Same process — drop `floodgate-spigot.jar` into `/plugins/`. Restart. Floodgate is what lets Bedrock players join without a Java account.

5. **Open the Geyser UDP port.** Geyser defaults to UDP port 19132. In the ExtraVM panel, make sure this UDP port is allocated and open. If the panel assigned you a different port range, edit `plugins/Geyser-Spigot/config.yml` and set `bedrock.port` to whichever UDP port you were assigned.

6. **Find your server addresses.** You'll have two:
   - **Java address**: your server IP (and port if not 25565) — give this to PC/Java players.
   - **Bedrock address**: your server IP + the Geyser UDP port (e.g., `play.yourserver.com:19132`) — give this to Xbox/PlayStation/Switch/mobile players. They add it under "Servers" in the Bedrock multiplayer menu.

7. **Test from both editions.** Join from Java first to confirm the server works. Then join from Bedrock (the easiest way to test is the Windows 10/11 Bedrock client, since you can type the IP directly). If Bedrock can't connect, 90% of the time it's the UDP port — check the panel and the Geyser config.

8. **Optional: install ViaVersion** if you want Java players on different client versions to all join. Drop it in `/plugins/`, restart.

That's the whole thing. From purchase to a working crossplay server is realistically 15-30 minutes if you've never done it before, faster if you have.

## Common Crossplay Problems and How to Fix Them

A few things come up over and over with crossplay servers:

- **"Bedrock players get 'could not connect'."** Almost always the UDP port. Geyser needs UDP, not TCP. Make sure the port is open in the host panel and that you gave Bedrock players the right port (the Geyser port, not the Java port).
- **"Bedrock players see a different world / missing blocks."** This is a Geyser translation limitation with some modded blocks. Vanilla and most plugins translate fine; heavily modded servers can have visual glitches for Bedrock players. There's no fix beyond reporting it to the Geyser project.
- **"Bedrock players get kicked for 'invalid session'."** Floodgate isn't installed or isn't running. Without Floodgate, Bedrock players need a Java account. With it, they use their Xbox Live identity.
- **"Server lags when Bedrock players join."** You're out of RAM. Geyser's per-player overhead is real. Upgrade a tier — ExtraVM lets you upgrade mid-cycle for a prorated charge, so you don't lose money.
- **"My console friend can't add the server."** Console Bedrock players (Xbox, PlayStation, Switch) need the server to be on a list of approved servers in some cases, or they need to use the "add server" option with the IP and port. This is a console-side restriction, not a host issue.

## What Real Users Say

ExtraVM has a 4.8/5 rating on Trustpilot based on customer reviews, and the recurring themes in long-term reviews (multiple 2+ year customers) are: stable performance, the in-house support team actually knowing what they're talking about, and the price-to-RAM ratio being competitive. The critical reviews that exist tend to be about specific incident responses rather than systemic issues — which is roughly what you'd expect for a host that's been around this long.

The Reddit consensus on r/feedthebeast and r/MinecraftServer is that ExtraVM is a solid mid-tier pick: not the absolute cheapest per GB you can find (sub-$1.50/GB hosts exist if you hunt), but the hardware (Ryzen 9 / i9, NVMe) and the in-house support justify the premium for people who don't want to babysit their server. For crossplay specifically, the things that matter — UDP port access, plugin freedom, DDoS protection, Paper support — are all there.

## Picking the Right Plan: A Quick Decision Guide

If you want to skip the analysis:

- **Just me and 2-3 friends, vanilla, mostly Java with maybe one Bedrock player:** 2 GB US/EU ($6/mo). 👉 [Get the 2 GB plan](https://bit.ly/Extravm).
- **A friend group of 8-15 mixed Java/Bedrock players, some plugins:** 4 GB US/EU ($12/mo). This is the most common pick. 👉 [Get the 4 GB plan](https://bit.ly/Extravm).
- **20-30 mixed players, moderate plugins, a small community server:** 6-8 GB US/EU ($18-24/mo). 👉 [Get the 6 GB plan](https://bit.ly/Extravm).
- **Modpack server (All The Mods, RLCraft, etc.) with crossplay:** 10-12 GB minimum, US/EU ($30-36/mo). Modpacks are RAM-hungry and Geyser adds on top. 👉 [Get the 10 GB plan](https://bit.ly/Extravm).
- **Most players are in Asia or Australia:** Use the Singapore or Sydney location at $5/GB. 👉 [Browse SG/AU plans](https://bit.ly/Extravm).

You can always start smaller and upgrade — ExtraVM does prorated mid-cycle upgrades, so there's no penalty for underestimating. The 5-day refund window also means if you genuinely can't get crossplay working, you can get your money back.

## Final Thoughts

Minecraft crossplay server hosting isn't complicated once you understand that "crossplay" really means "Java server + GeyserMC + Floodgate + a host that doesn't block UDP." The hard part isn't the plugins — those install in minutes. The hard part is picking a host that gives you the freedom to actually run them: open ports, install arbitrary plugins, run Paper, and not get DDoSed off the internet the moment your server shows up on a public list.

ExtraVM checks those boxes and has been doing it long enough that the rough edges are mostly sanded down. The $3/GB US/EU pricing is fair for the hardware you get, the in-house support matters more than people realize until they need it, and the 5-day refund means trying it is low-risk. If you're setting up your first crossplay server for a mixed-device friend group, the 4 GB US/EU plan is where I'd start — 👉 [you can grab it here](https://bit.ly/Extravm).

Get Geyser and Floodgate installed, hand your Bedrock friends the IP and port, and you'll have everyone in the same world by tonight.
