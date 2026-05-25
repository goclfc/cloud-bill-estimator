# cloud bill estimator

day-1 dev tool by usectl. single-file html, deployable on usectl as a static site in 60 seconds.

## what it does

user fills in their app config (compute size, bandwidth, db size, storage, addons, team seats). instantly shows expected monthly bill on:

- vercel (pro plan, metered)
- railway (pro plan, usage-based)
- render (team plan, per-resource)
- usectl (flat tier, everything included)

result card highlights the cheapest. savings vs. average of others. "share on x" button drives traffic back to usectl.com.

## how the pricing models work

all estimates use 2026 published pricing. usectl pricing is flat tiers:

- **small ($40):** 0.5 vCPU, 1GB RAM, all addons + seats + bandwidth included
- **medium ($100):** 1 vCPU, 2GB RAM, all addons + seats + bandwidth included
- **large ($250):** 2 vCPU, 4GB RAM, all addons + seats + bandwidth included

competitor pricing models meter:
- vercel: $20/seat + bandwidth overage ($40/100GB) + functions + addons
- railway: $20/seat + $10/vCPU + $10/GB RAM + $0.10/GB egress + addons
- render: $7-85 base by tier + $19/seat + $30/100GB overage + addons

**these are estimates**, not quotes. the disclaimer in the UI says this.

## customize before deploy

three things to verify before you ship:

1. **usectl pricing tiers.** i used $40 / $100 / $250 as placeholders. swap in the real numbers in `priceUsectl()` if they differ.
2. **competitor pricing factors.** the math inside each `priceX()` function is a best-effort approximation. spot-check against current public pricing pages before announcing.
3. **og image.** the og tags are basic. you may want a real og image for x sharing (1200x630 png).

## deploy

```bash
# 1. push to a new github repo
cd /Users/<you>/path-to/cloud-bill-estimator
git init -b main
git add index.html README.md
git commit -m "cloud bill estimator v1"
gh repo create cloud-bill-estimator --public --source=. --push

# 2. create the machine
usectl machines create \
  --name bills \
  --repo https://github.com/<you>/cloud-bill-estimator \
  --branch main \
  --domain bills \
  --type static

# 3. live at https://bills.usectl.com
```

if you want it at `usectl.com/tools/cloud-bill`, put it behind your existing site routing.

## share on x

post template:

```
shipped a quick free tool today.

cloud bill estimator. paste your app config, see what you'd pay
on vercel + railway + render + usectl. side by side, no signup.

honest comparison. usectl wins where flat pricing wins,
loses where it doesn't.

https://bills.usectl.com
```

attach a screenshot of the tool with a meaty result (medium app + 500GB bandwidth + postgres = usectl wins by $50+/mo).

## tracking

set up before posting:
- google analytics or plausible on the page
- track conversion to usectl.com (link from footer)
- track "share on x" button clicks
- track which compute size + addons get selected most (informs future pricing)

## what to ship next

if this tool gets traction (~1000+ visits), ship the next ones from the `usectl-tools/` plan:

1. **agent-readiness checker** — paste github url, get a score
2. **marketing budget calculator** — found-friendly, broader audience
3. **one-liner optimizer** — pitch deck adjacent

each one extends the brand surface and feeds the funnel.

## maintenance

prices in the source change quarterly. set a calendar reminder to spot-check this tool against current pricing every 3 months. otherwise the estimates go stale and credibility erodes.
