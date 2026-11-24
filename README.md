# CyberPanel VPS: What You Pay For, Which Evoxt Plan Fits, and What Breaks After Install (Every Plan Priced)

CyberPanel is free. The server is not. On Evoxt, the plan that makes sense for most people is **VM-1 at $5.99/month**: 1 vCPU, 2 GB RAM, 20 GB storage, 500 GB monthly transfer. That is the cheapest Evoxt plan meeting CyberPanel's own recommendation of 2 GB RAM for production use. The $4.99 VM-0.75 with 1 GB will install and run the panel, but its 10 GB disk sits exactly on CyberPanel's stated minimum, so you start with no headroom.

Below: every Evoxt plan and price, where the free tier of CyberPanel stops, how much RAM each kind of site really needs, and the handful of things that go wrong in the first week.

> **Quick pick:** VM-1, $5.99/month. One WordPress site plus mail plus object caching fits. Take VM-0.75 ($4.99) only for a test box you don't mind rebuilding.
> 👉 [See all Evoxt plans and current monthly prices](https://bit.ly/EvoXt)

## Every Evoxt plan, priced

| Plan | vCPU / RAM | Storage | Monthly transfer | Price | Order |
| --- | --- | --- | --- | --- | --- |
| VM-0.5 | 1 core / 512 MB | 5 GB | 250 GB | $2.99/mo | [Order VM-0.5](https://bit.ly/EvoXt) |
| VM-0.75 | 1 core / 1 GB | 10 GB | 250 GB | $4.99/mo | [Order VM-0.75](https://bit.ly/EvoXt) |
| VM-1 | 1 core / 2 GB | 20 GB | 500 GB | $5.99/mo | [Order VM-1](https://bit.ly/EvoXt) |
| VM-1.5 | 2 cores / 2 GB | 20 GB | 500 GB | $6.95/mo | [Order VM-1.5](https://bit.ly/EvoXt) |
| VM-2 | 2 cores / 4 GB | 30 GB | 1,000 GB | $11.99/mo | [Order VM-2](https://bit.ly/EvoXt) |
| VM-3 | 4 cores / 4 GB | 30 GB | 1,000 GB | $14.99/mo | [Order VM-3](https://bit.ly/EvoXt) |
| VM-4 | 4 cores / 8 GB | 60 GB | 2,000 GB | $23.99/mo | [Order VM-4](https://bit.ly/EvoXt) |
| VM-6 | 8 cores / 8 GB | 60 GB | 2,000 GB | $29.99/mo | [Order VM-6](https://bit.ly/EvoXt) |
| VM-8 | 8 cores / 16 GB | 80 GB | 3,000 GB | $47.99/mo | [Order VM-8](https://bit.ly/EvoXt) |
| VM-12 | 16 cores / 16 GB | 80 GB | 3,000 GB | $60.95/mo | [Order VM-12](https://bit.ly/EvoXt) |
| VM-16 | 16 cores / 32 GB | 100 GB | 5,000 GB | $95.99/mo | [Order VM-16](https://bit.ly/EvoXt) |

All plans include weekly offsite backups at no extra cost, full root access, a browser VNC console, and an IPv6 address. Billing runs monthly up to a three-year prepay, and you can top up account credit instead. Evoxt lists credit card, debit card, PayPal, Bitcoin and USDt Tron as payment methods. The published CPU figure is up to 6.0 GHz turbo, but for CyberPanel the RAM and disk columns decide whether the thing is pleasant or painful. Prices above are Evoxt's published monthly rates; confirm the current number on the deploy page before you pay.

On CyberPanel specifically: Evoxt documents a one-click install with no commands, after which you go straight to `https://your-server-ip:8090`. That documentation is a few years old, so check that CyberPanel still appears in the image list when you deploy. If it doesn't, the install script takes about ten minutes and is covered below.

## What the free version of CyberPanel actually covers

CyberPanel is open source and built around OpenLiteSpeed, which is why it shows up in WordPress conversations: LiteSpeed Cache runs at the server level rather than as a PHP plugin. A default install also handles mail (Postfix and Dovecot), DNS, SSL certificates through Let's Encrypt with automatic renewal, backups, file management and a firewall. Docker can be installed and managed from inside the panel.

The free tier bundles OpenLiteSpeed. LiteSpeed Enterprise, which swaps in the commercial web server, is a separate paid upgrade, and a few add-on modules are sold separately too. For a single site or a handful of them, the free stack is the one you want.

The bill you actually pay is made of three parts: the VPS, the time you spend administering it, and email deliverability. CyberPanel is unmanaged by nature. Nobody is going to notice your disk filling up except you. If you run your own mail server, expect to spend an afternoon on SPF, DKIM and DMARC before Gmail trusts you, and consider routing outbound mail through a dedicated SMTP provider anyway.

## Preinstalled image or manual install

A preinstalled CyberPanel image is the shortest path. Hosts selling it as a template typically hand over a working panel within minutes of payment, and the trade-off is that switching to or away from the template wipes the disk, so back up first. Manual installation on a clean server gives you the same result with one extra skill: pasting a command.

The requirements are not demanding but they are specific:

- **OS:** Ubuntu 18.04, 20.04 or 22.04; AlmaLinux 8 or 9; CloudLinux 8
- **RAM:** 1 GB minimum, 2 GB or more for production
- **Disk:** at least 10 GB free
- **Server state:** a fresh install, so nothing conflicts with the stack

The official script asks a series of questions. Installation type `1` starts it, web server `1` picks OpenLiteSpeed, full service `Y` installs DNS, FTP and mail so you don't configure them later, and Enter takes the latest stable version. Choosing `S` lets you set your own admin password instead of accepting a random one, then `Y` for Memcached, `Y` for Redis and `Y` for WatchDog.

When it finishes, the panel lives at `your-server-ip:8090`, the default username is `admin`, and the password is whatever you set. Your browser will warn that the certificate is untrusted. That is expected, because the panel is served from a bare IP with no HTTPS. It is also the one warning you should not train yourself to click through on other sites.

Four things to do on day one:

1. Open only ports 22, 80, 443 and 8090 in the firewall.
2. Turn on ModSecurity.
3. Change the default passwords on both the panel at `:8090` and the OpenLiteSpeed admin console at `:7080`.
4. Keep the panel and system packages updated.

## How much RAM your site needs

CyberPanel's 2 GB recommendation is not padding. The panel, OpenLiteSpeed, MySQL or MariaDB, and a mail server all sit in memory before your first visitor arrives.

**1 GB (VM-0.75, $4.99):** one small site, or a staging box. Fine for learning the panel and testing a theme. A WordPress site with a few plugins, mail and Redis on 1 GB will swap under any real traffic, and the 10 GB disk leaves nothing for backups.

**2 GB (VM-1, $5.99; VM-1.5, $6.95):** the production floor. VM-1 gives you the RAM with one core; VM-1.5 adds a second core for $0.96 more, which matters if PHP and MySQL are competing for CPU during traffic spikes. For a brochure site or a blog, VM-1 is enough. For WooCommerce, take the second core.

**4 GB (VM-2, $11.99; VM-3, $14.99):** several sites on one panel, heavier plugins, or a Docker container or two alongside the web stack. VM-3 doubles the cores for $3 more, which is the better buy of the pair if you're running anything concurrent.

**8 GB and up (VM-4, $23.99 onward):** multi-container setups, Git-driven deployments pulling from GitHub or GitLab on webhook, or agency work where you host client sites under separate accounts.

👉 [Check the VM-1 plan at $5.99/month](https://bit.ly/EvoXt)

## Five things that bite after install

**Your .htaccess rules don't behave the same.** OpenLiteSpeed reads most rewrite rules, but it needs a restart to pick up changes, so plugins that write to .htaccess can behave differently than they would on Apache. If a plugin claims to need Apache, test it before you commit.

**Installing a different OS or panel template erases the disk.** Any control-panel switch at the provider level is a reinstall. Take a backup, and expect to reconfigure the panel afterwards.

**The untrusted-certificate warning is normal, but only here.** The panel runs on a naked IP without HTTPS. Accepting that once for `:8090` is fine. Accepting it on a customer's storefront is not.

**Outbound mail lands in spam.** A fresh IP with no sending history and no SPF, DKIM or DMARC records will get filtered. Set up the DNS records, or relay through a transactional SMTP service.

**Default credentials stay default.** The `admin` account on `:8090` and the OpenLiteSpeed console on `:7080` both need new passwords, because scanners look for exactly those two ports.

## What other hosts charge for the same job

Useful anchors, all published list prices and not apples-to-apples comparisons:

| Provider | Plan | Price |
| --- | --- | --- |
| CyberPanel Cloud VPS | 3 vCPU / 8 GB / 90 GB NVMe | $14.99/mo or $0.021/hr |
| CyberPanel Cloud VPS | 4 vCPU / 16 GB / 240 GB NVMe | $23.99/mo |
| ARPHost | 1 vCPU / 1 GB / 50 GB NVMe, CyberPanel preinstalled | $4.25/mo |
| ARPHost | 2 vCPU / 8 GB / 75 GB NVMe | $5.99/mo |

Two things worth noticing. First, at the 8 GB mark the spread is wide: ARPHost lists $5.99/month for 2 vCPU and 8 GB, while CyberPanel's own Cloud VPS charges $14.99 for 3 vCPU and 8 GB. CyberPanel's marketing compares itself to DigitalOcean and Vultr at $48/month for 8 GB, which is a real gap but not the cheapest end of the market. Second, Evoxt doesn't compete on RAM per dollar at the low end; it competes on single-core clock speed and on bundling weekly backups into every plan. If raw memory is your only metric, VM-1 loses to ARPHost's 8 GB offer. If you want a fast single core and a backup you didn't have to configure, the comparison changes.

Disk, transfer allowance, data-centre location and whether CyberPanel comes preinstalled all differ between these plans, so line up the specs before you line up the prices.

👉 [Compare every Evoxt plan side by side](https://bit.ly/EvoXt)

## Picking your plan

Match the box to the job:

- **Testing CyberPanel, or a single low-traffic site:** VM-0.75 at $4.99. Accept that 1 GB and 10 GB are the floor and plan to move up.
- **One WordPress site with mail and caching:** VM-1 at $5.99. Add VM-1.5 at $6.95 if the site takes orders.
- **Several sites, or a container or two:** VM-2 or VM-3, $11.99 to $14.99.
- **Client hosting, multiple containers, Git deploys:** VM-4 and above.

Two things to confirm before checkout: whether CyberPanel shows up in the image list at deploy time, and which billing term you're signing up for, since Evoxt sells anything from monthly to a three-year prepay. Nothing in the panel costs extra either way, but a longer term is only a good deal if you're staying.

👉 [Start with Evoxt's CyberPanel one-click install](https://bit.ly/EvoXt)
