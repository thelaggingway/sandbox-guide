# Styne research ledger

Provider ID `styne`. Desk research only, at repo revision `fa86e49` (= `origin/main`), checked 2026-11-26. No Styne account, key or sandbox was created.

## How this was produced

Five research packages ran in parallel Amp threads, each in a1.tiny orbs, and this ledger joins their claim-level evidence unchanged. The primary thread then reconciled the verdicts, wrote the rate card and worked A/B/C by hand.

| Package | Thread | Mode |
|---|---|---|
| 1 Machine and money | https://ampcode.com/threads/T-01a2193c-9dae-7370-b1b7-ce6d6d847d26 | dsf41-neuralwatt |
| 2 State and staying on | https://ampcode.com/threads/T-01a2193c-a6b3-7705-b3c7-2d3f6d41f1d4 | dsf41-coralbricks |
| 3 Access | https://ampcode.com/threads/T-01a2193c-af8d-717b-b8fa-766921c08ae8 | dsf41-singularity |
| 4 Running and company | https://ampcode.com/threads/T-01a2193c-b5d2-71e2-8eb7-65efad5f4e62 | dsf41-neuralwatt |
| 5 Browsers and search | https://ampcode.com/threads/T-01a2193c-bf30-74ef-9f61-21a4c18915e5 | dsf41-coralbricks |

Retrieval: TinyFish `fetch_content` (ttl 0) and Context.dev `web_scrape` (maxAgeMs 0) through the connected MCP tools for quotes, with direct `curl` of the raw `.md` pages as the byte check. An Exa fetch early on returned a stale cached limits page, so Exa was not used for prices. Context.dev's changelog copy was also stale (no 2026-11-26 entry). `llms-full.txt` holds only page intros, so grepping it proves nothing.

## Integration decisions (primary thread)

- `browser`: **partly**, not yes. The column means a browser in the provider's own machine, and Styne's is a separately metered hosted product (P5's revision).
- `desktop`: partly. noVNC is exposed only through the Boat-compatible routes.
- `dashboard-create`: partly. The dashboard starts browsers, but no page shows it creating a VM.
- `docker-inside`: partly. Docker is documented only in the CI runner image.
- `stay-on`: partly. One timeout lasts at most 86,400 s, so a month needs a keep-alive loop calling /refreshes or /timeout. Case B is priced as feasible, with a flag.
- Search API: not given its own section or a use-case mention. It is mid-priced, and the "own index" claim is uncorroborated (the docs show `web:brave`).
- Situations: one pick, in `browser-agents`. Use cases: routes for all 10, plus one pick ("Browsers that stay logged in") in `browser-agent`. No one-click or preinstalled route exists for Hermes, OpenClaw or n8n, so those three are `manual`.

## A/B/C (hand-worked, 730 h month)

Starter is a $20 fee with 700 VM hours counted at the 4 vCPU / 8 GB size, which is $22.40 at $0.032.
- **A:** small. 166.67 h × $0.016 = $2.67, inside the allowance, so **$20.00**.
- **B:** default. 730 h × $0.032 = $23.36, which is $0.96 over the $22.40, so **$20.96**. This needs a keep-alive loop.
- **C:** small. 90 h × $0.016 = $1.44, and the 640 paused hours are $0, so **$20.00**.

The engine (`price()`) picks the same plan and shapes, and its machine-time line items match: $2.67, $23.36 and $1.44.

Model limitations: the stated $0 for paused time conflicts with "metered at its pause tier's rate", for which no rate is published. Egress is `null`. Browser hours are not priced by the calculator.


---

<!-- package file: p1-machine-money.md -->
## Styne — package 1, machine and money

Research worker ledger for adding provider `styne` to BotPerch.
Repository `thelaggingway/sandbox-guide` at revision `fa86e49` (= `origin/main`), worktree otherwise clean.
Scope: features `isolation`, `sizes-regions`, `billing-models`, `trials-free-tiers` and their 19 capabilities.
Desk research only: no Styne account was created, no key was used, nothing billable was started.

All quotes below were re-fetched on **2026-11-26** and machine-checked against the fetched page text. Normalisation
used for the check: markdown emphasis (`**`), inline code backticks and link markup removed (a link becomes its text),
backslash escapes removed (Mintlify writes `\$` and `\_`), all whitespace runs collapsed to one space, outer whitespace
trimmed. Nothing else was changed: numbers, units and qualifiers are as published.

## Retrieval routes (what actually ran)

| Route | Calls | Result |
|---|---|---|
| TinyFish MCP `fetch_content` | `docs.styne.ai/llms.txt`, `index.md`, `sandboxes.md`, `regions.md`, `pause-tiers.md`, `images.md`, `billing.md`, `limits.md`, `exec.md`, `guide-templates.md`, `changelog.md`, `agent-signup.md`, `ci.md`, `styne.ai/openapi.json` | All succeeded; this is the route behind every documentation quote below |
| Context MCP `web_scrape` | `styne.ai/pricing`, `styne.ai/sandboxes`, `styne.ai/legal/subprocessors` | All succeeded; this is the route behind the three site quotes below |
| Direct HTTPS GET (`curl`, documented as a sweep, not as evidence) | `llms.txt`, `llms-full.txt`, all 178 `.md` pages linked from `llms.txt`, `styne.ai/openapi.json` | Used only to grep the whole docs set for `docker`, `gpu`, `kvm`, `nested`, `resize`, `kernel`, `card`, `trial`. Every quote used in a row was then re-read through TinyFish/Context |
| Failed | none | No Amp-native `web_search`/`read_web_page` fallback was needed and none was used. Exa was not used |

Freshness notes:

- `llms-full.txt` is **not** the full text: it concatenates each page's one-line intro. Grepping it for `microVM` or
  `GPU` proves nothing. The page-level sweep above was done on the individual `.md` pages instead.
- The docs sit behind Cloudflare. `llms-full.txt` answered `cf-cache-status: HIT, age: 634`; the `.md` pages answered
  `cache-control: no-store` but still reported `cf-cache-status: HIT` with ages up to ~52 minutes. The rows therefore
  rely on the TinyFish/Context fetches (which returned origin content, including changes dated today) rather than the
  CDN copy.
- The parent's warning about an Exa cache serving old plan names (Builder/Startup) is partly a live-page problem, not
  only a cache problem: the current `limits.md` "Search, fetch and crawl" table still says "2,000 Builder; 5,000
  Startup; 10,000 Scale" while the plans themselves are Starter/Pro/Scale. Recorded as contradiction 3.

## Verdict summary

| Capability | Verdict | Figure | One-line reason |
|---|---|---|---|
| own-vm | yes | own kernel microVM | A microVM per sandbox with its own kernel, disk and network |
| root | yes | root by default | Commands run as root unless the call names another template user |
| docker-inside | partly | Docker in CI image | Docker with Buildx is documented for CI runners, not for sandbox templates |
| nested-virt | not-published | none published | No page, size or API field mentions /dev/kvm or nested VMs |
| custom-image | partly | public registries only | Public images from 3 registries only, ≤10 GiB, private images refused, no Dockerfile build |
| any-size | no | 4 fixed sizes | Four shapes; cpuCount/memoryMB round up to the smallest shape that has them |
| big-machines | partly | 16 vCPU / 32 GiB, Scale | The big shape exists only on the $499 Scale plan |
| live-resize | no | fixed at create | No resize operation; a sandbox keeps its template's size |
| gpu | not-published | no GPU size | No GPU on any size, plan, price line or API field |
| choose-region | partly | Ashburn only | One live region; `ams` is accepted but served in Ashburn |
| eu-region | no | Amsterdam dark | The only EU region is "coming back later" |
| per-second | yes | per second | Metered per second while it runs |
| active-cpu | partly | full rate while running | Whole size rate while any of your processes run; a sandbox with none sleeps free |
| cheap-pause | yes | paused $0 | Paused and sleeping VMs cost $0 today |
| no-monthly-fee | no | Starter $20/mo | Every usable path needs a $20/$99/$499 subscription |
| flat-price | no | hourly by size | Flat per account, not per machine; overage bills by size |
| free-tier | no | 300 s once | Nothing recurs monthly; 300 free seconds once, on an agent-created account |
| free-credit | partly | 20 VM hours on trial | 7-day trial hours, card required, no dollar credit |
| no-card | partly | card for the trial | Email signup and 300 free seconds without a card; a usable sandbox needs the trial's card |

## Capability evidence

### own-vm — yes

- URL: https://styne.ai/sandboxes
- Quote: "Each sandbox has its own kernel, disk and network. Nothing is shared with another task."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: Context `web_scrape`
- Note for the row: each sandbox is a microVM with its own kernel, disk and network; the CI page says a runner is
  "isolated like any other Styne VM: its own kernel, no access to the host or to other VMs". No page names the
  hypervisor (it is not called Firecracker anywhere in the docs), so the row does not claim one.
- Related quote (not the row's source): https://docs.styne.ai/sandboxes.md — "A sandbox is a microVM restored from a
  template snapshot that you create, run commands in, pause, fork and kill."

### root — yes

- URL: https://docs.styne.ai/exec.md
- Quote: "`user` (a user that exists in the template; the default is root)"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Note: the base image has `/root` as home and `sudo` installed; a call that names another user runs as that user.

### docker-inside — partly

- URL: https://docs.styne.ai/ci.md
- Quote: "The Docker daemon starts by itself when the job starts (about a second), so `docker build`, `docker run` and
  `docker compose` work in the first step without `sudo dockerd &`"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): Docker with Buildx is documented only in the CI runner image, where the daemon starts with the job;
  no sandbox template ships Docker and no sandbox page says a daemon can run.
- Reasoning: the sentence sits in the CI runner section ("A job is customer code, so a runner is isolated like any
  other Styne VM"). The sandbox templates (`small`/`default`/`large`/`xlarge`) are listed with "Python 3, Node, git,
  curl" and no Docker. A full VM with root could probably install it, but that is unpublished, so the capability is
  `partly` rather than `yes`.

### nested-virt — not-published

- URL: https://styne.ai/openapi.json
- Quote: "Isolated Linux sandboxes for AI agents: create from a template, run commands, files, fork, snapshots, pause
  and resume."
- Confidence: `not-published` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Note (row): no page, plan, size or API field mentions `/dev/kvm` or nested virtual machines; the OpenAPI document
  lists 69 paths and none is a virtualisation option.
- Search done: all 178 doc pages fetched and grepped for `kvm` and `nested`; the only hits are a JSON "nested" path in
  `fetch.md`. Nothing was found, so this stays `not-published`, not `no`.

### custom-image — partly

- URL: https://docs.styne.ai/images.md
- Quote: "Public images on Docker Hub (`docker.io`), `ghcr.io` and `quay.io` work; any other registry answers `422
  image_registry_not_allowed`."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): only public images on Docker Hub, ghcr.io and quay.io start, at most 10 GiB unpacked; private images
  are refused with 401 and there is no Dockerfile build — you install into a sandbox and save it as a snapshot.
- Note (row): a new image is prepared on first use (20–60 s, answers 202 `image_building`); five images are always
  ready; `linux/amd64` only; 20 new images an hour per account.

### any-size — no

- URL: https://docs.styne.ai/limits.md
- Quote: "A create that names no template and no size gets `default`; one that names only `cpuCount` or `memoryMB` gets
  the smallest size that has them."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): you choose one of four shapes (2/4, 4/8, 8/16, 16/32); naming a CPU or memory number only rounds
  you up to the smallest shape that has it.
- Note (row): a sandbox takes the size of its template and your own templates keep the size they were built with;
  asking a template for more than it has answers 400.

### big-machines — partly

- URL: https://docs.styne.ai/billing.md
- Quote: "| Largest size | 8 vCPU, 16 GB | 8 vCPU, 16 GB | 16 vCPU, 32 GB |"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): the 16 vCPU / 32 GiB xlarge is sold only on the $499 Scale plan; Starter and Pro stop at 8 vCPU /
  16 GB.
- Note (row): xlarge carries 251 GiB of disk; the size table is the same on every plan, the plan caps which shape you
  may start.

### live-resize — no (inferred)

- URL: https://docs.styne.ai/limits.md
- Quote: "A sandbox takes the size of its template; your own templates keep the size they were built with."
- Confidence: `inferred` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): a sandbox keeps the size of its template or snapshot and nothing resizes it; the API reference has
  no resize call and a volume "cannot be resized" either.
- Reasoning: the OpenAPI document (69 paths) and the API reference contain no resize operation for a sandbox; the only
  `resize` in the docs changes a PTY's rows and columns, or is refused for volumes. No page states the refusal in
  words, so this is an inference from the complete operation list, tagged `inferred` rather than `docs-confirmed`.

### gpu — not-published

- URL: https://styne.ai/pricing
- Quote: "Sizes up to 16 vCPU and 32 GB"
- Confidence: `not-published` · checked 2026-11-26 · route: Context `web_scrape`
- Note (row): no size, plan, price line or API field mentions a GPU; every published shape is 2 to 16 vCPU with 4 to
  32 GiB and no accelerator, and every price is a VM hour.
- Search done: `gpu` appears on none of the 178 doc pages; the pricing page's size table and the limits size table are
  both complete and CPU-only. Silence is recorded as `not-published`.

### choose-region — partly

- URL: https://docs.styne.ai/regions.md
- Quote: "A VM or browser runs in US East (Ashburn). Amsterdam (EU) is coming back later; a create that names ams runs
  in Ashburn and the answer's region says ash."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): one region is live (US East, Ashburn); `ams` is accepted but served in Ashburn and the answer says
  `ash`, so naming a region does not move the machine.
- Note (row): `GET /regions` reports `available` per region and the dashboard picker reads the same list; `eu`,
  `eu-central`, `us-east` and `ashburn` are accepted as aliases; snapshot and volume starts stay in their own region.

### eu-region — no

- URL: https://docs.styne.ai/regions.md
- Quote: "| Europe (Amsterdam) | `ams` | Coming back later. Accepted, served in Ashburn. |"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): the only EU region, Amsterdam, is "coming back later": a create that names `ams` runs in Ashburn.
  Nothing an EU buyer can select keeps data in the EU today.
- Note (row): the sub-processor page still lists Hostkey in the Netherlands for sandbox servers — see contradiction 1.

### per-second — yes

- URL: https://docs.styne.ai/billing.md
- Quote: "A sandbox is metered per second while it runs."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Note (row): the meter runs from create to kill, timeout or pause; a paused or sleeping sandbox accrues no seconds.
  Plan hours are counted at the 4 vCPU / 8 GB size, so a small sandbox spends half an hour of allowance an hour.

### active-cpu — partly

- URL: https://docs.styne.ai/pause-tiers.md
- Quote: "The platform puts a running sandbox to sleep only when nothing of yours runs inside it (no process started
  after the sandbox was created: no server, job or open shell). Such a sandbox costs nothing to keep and wakes in
  milliseconds on the next command, file call or connection, so there is nothing to notice."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): a running sandbox pays its whole size rate whether its CPU is busy or idle; only a sandbox with no
  process of yours inside is slept automatically, and that one costs nothing.
- Note (row): shared vCPUs run at a 16 % baseline (12 % at 1 vCPU, 23 % at 16) with burst credit, and `guaranteed`
  whole cores are for paid accounts. The meter is seconds at the size rate, not measured CPU.
- Supporting quote: https://docs.styne.ai/limits.md — "It may use **16 % of each vCPU** without limit (its baseline;
  12 % for a 1 vCPU size and 23 % for 16 vCPUs) and earns credit for every second it stays under it."
- Supporting quote: https://docs.styne.ai/sandboxes.md — "`guaranteed` gives the sandbox whole cores of its own, for
  paid accounts"

### cheap-pause — yes

- URL: https://docs.styne.ai/billing.md
- Quote: "A paused or sleeping VM or browser costs $0."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Note (row): the billing page and the pricing page both say a paused VM costs $0, and a memory pause keeps its memory
  on the node without a charge. The same billing page also says a paused sandbox is metered at its pause-tier rate and
  no tier price is published — see contradiction 2.
- The pricing page repeats it: https://styne.ai/pricing — its "Every limit" table row "A sleeping browser or paused
  VM" reads $0 in all three plan columns, and its sandbox price block shows "$0" under the heading "an hour while
  paused" with the line "State kept, nothing billed".
- Caveat a buyer should know: a `memory` pause still counts against the concurrent-VM limit and against "sandboxes
  resident on the nodes"; `standby` and `archived` pauses do not. Storage is free today: volumes and saved snapshots
  are "billed at the same stored-GiB rate as saved snapshots, which is $0 today".

### no-monthly-fee — no

- URL: https://docs.styne.ai/billing.md
- Quote: "Styne Starter ($20 a month) starts with a 7-day free trial (card required); Styne Pro ($99 a month) and Styne
  Scale ($499 a month) are charged at checkout."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): every way to run a sandbox past the free 300 seconds needs a subscription: Starter $20, Pro $99 or
  Scale $499 a month. There is no pay-as-you-go-only account.
- Note (row): the hourly overage rates ($0.016–$0.128 a VM hour) apply only inside a plan, after its included hours.

### flat-price — no

- URL: https://styne.ai/pricing
- Quote: "Then $0.032 per VM hour (4 vCPU default; small 2 vCPU $0.016, large $0.064, xlarge $0.128) and $0.04 per
  browser hour"
- Confidence: `pricing-page` · checked 2026-11-26 · route: Context `web_scrape`
- Shortfall (row): the subscription is flat per account, not per machine, and hours past the allowance bill by the
  size of the VM, so a busy month still moves the bill.
- Note (row): the pricing page's "Per month" column ($12 / $23 / $47 / $93) is 730 hours at the hourly rate, not a
  flat machine price.

### free-tier — no

- URL: https://docs.styne.ai/agent-signup.md
- Quote: "| Free sandbox time | 300 seconds in total, then `429 free_tier_exhausted` with the way up (add a card in the
  dashboard) |"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): nothing recurs for free: an account created by an agent gets 300 seconds of sandbox time in total,
  and a person's account needs the 7-day trial or a plan.
- Note (row): the 7-day trial's 20 VM hours are a one-off; after it the cheapest plan is $20 a month.

### free-credit — partly

- URL: https://docs.styne.ai/billing.md
- Quote: "The 7-day trial of the Starter plan has smaller limits: 3 VMs and 2 browsers at once, 150 new a day, 20 VM
  hours and 20 browser hours, 10 saved snapshots."
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): the starting allowance is 20 VM hours and 20 browser hours inside a 7-day trial that needs a card;
  there is no dollar credit and the hours stop at 20 unless you upgrade.
- Note (row): 20 VM hours at the default size is $0.64 of compute at the overage rate; a trial that reaches 20 hours
  answers 422 `limit_hours`.

### no-card — partly

- URL: https://docs.styne.ai/agent-signup.md
- Quote: "To go beyond the 300 free seconds, the owner starts the 7-day trial or picks a plan under Billing; a card is
  required"
- Confidence: `docs-confirmed` · checked 2026-11-26 · route: TinyFish `fetch_content`
- Shortfall (row): you can sign up with an email address and no card, and an agent can open an account with 300
  seconds of free sandbox time; a person's first usable sandbox needs the 7-day trial, and that needs a card.
- Note (row): an account with no trial or plan answers 402 `plan_required` on create; the trial itself is "7 days,
  card required".
- Supporting quote: https://docs.styne.ai/authentication.md — "Sign up with an email address, then create a key on the
  **API keys** page of the dashboard." (no card mentioned for signing up)

## Feature rows

### isolation — yes

- Figure: microVM, own kernel
- Scenario: "Docker is not in the sandbox image, so that step fails unless you install the engine yourself; the
  boundary is a microVM with its own kernel, but no page says whether a sandbox may load a kernel module."
- Note: "A microVM per sandbox with its own kernel, disk and network. Docker with Buildx is documented only for CI
  runners, and no page says whether a sandbox may load kernel modules or use `/dev/kvm`. Images are public ones only
  from docker.io, ghcr.io and quay.io, up to 10 GiB unpacked."
- Source: https://styne.ai/sandboxes — "Each sandbox has its own kernel, disk and network. Nothing is shared with
  another task." (`docs-confirmed`)

### sizes-regions — partly

- Figure: 16 vCPU / 32 GiB, Scale
- Scenario: "2 vCPU with 16 GB does not exist: the four shapes are fixed at a 1:2 ratio, so the nearest is 8 vCPU /
  16 GiB, and no EU region is live — `ams` runs in Ashburn."
- Note: "Four sizes: 2/4, 4/8, 8/16 and 16/32 with 51, 51, 111 and 251 GiB of disk; 16 vCPU / 32 GiB only on Scale.
  One live region, US East (Ashburn); Amsterdam is 'coming back later' and `ams` is served in Ashburn. The pricing
  page says GB where the limits page says GiB."
- Source: https://docs.styne.ai/limits.md — "| `xlarge` | 16 | 32 GiB | 251 GiB |" (`docs-confirmed`)

### billing-models — partly

- Figure: $0.032/h + $20 plan
- Scenario: "A 2 vCPU / 4 GB box up for 730 hours uses 365 of Starter's 700 counted hours, so the month costs the $20
  plan and nothing more; the meter is seconds at the size rate, not measured CPU, and a paused box costs $0."
- Note: "Per second at $0.016 / $0.032 / $0.064 / $0.128 an hour by size, after included hours counted at the 4 vCPU /
  8 GB size (700 / 4,000 / 14,000 a month). Plans are $20 / $99 / $499. Paused and sleeping VMs and browsers cost $0;
  volumes and snapshots are $0 today. One billing sentence says a paused sandbox is metered at its pause tier's rate,
  which contradicts the $0 line."
- Source: https://docs.styne.ai/billing.md — "VM hours are counted at the 4 vCPU / 8 GB size: an hour on small uses
  half an hour, on large two, on xlarge four." (`docs-confirmed`)

### trials-free-tiers — partly

- Figure: 7-day trial, card
- Scenario: "The 7-day trial needs a card and stops at 20 VM hours; on day 8 the cheapest way to keep the agent up is
  Starter at $20 a month with 700 included hours. Without a card, an agent can create an account with 300 seconds in
  total."
- Note: "Starter has a 7-day trial with 3 VMs, 2 browsers and 20 VM hours, card required; Pro and Scale have no trial.
  An agent-created account gets 300 free sandbox seconds and a claim link, and its key expires after a day if nobody
  claims it. No free plan: an account with no trial or plan answers 402 `plan_required`."
- Source: https://docs.styne.ai/billing.md — "| Free trial | 7 days, card required | No | No |" (`docs-confirmed`)

## Contradictions (both sides kept)

1. **EU location: region page vs sub-processor page.**
   - https://docs.styne.ai/regions.md — "Europe (Amsterdam) | `ams` | Coming back later. Accepted, served in Ashburn."
     and "While Amsterdam is dark, every create runs in Ashburn, and `region` in the answer says so."
   - https://styne.ai/legal/subprocessors (last updated 12 November 2026) — "Hostkey Bare-metal servers that run
     sandboxes Everything inside sandboxes while they run or are paused Netherlands".
   - Interpretation used: the region page is the product statement a buyer acts on, so `eu-region` is `no` and
     `choose-region` is `partly`. Consequence: EU data residency cannot be selected today, but the sub-processor page
     says sandbox content is processed in the Netherlands, so an EU buyer's data may already leave the US — the two
     pages cannot both be current. Worth asking Styne directly.
   - Supporting detail that the EU capacity existed: the sandboxes page's measurement note reports Docker image start
     times "643 to 879 ms in ams", so `ams` was live recently.

2. **Paused billing: $0 vs metered at the pause-tier rate.**
   - https://docs.styne.ai/billing.md — "A paused or sleeping VM or browser costs $0."
   - https://docs.styne.ai/billing.md — "A paused sandbox is metered at its [pause tier](https://docs.styne.ai/pause-tiers.md)'s
     rate." and https://docs.styne.ai/pause-tiers.md — "A paused sandbox is metered at its tier's rate."
   - https://styne.ai/pricing — "| A sleeping browser or paused VM | $0 | $0 | $0 |" and "$0 an hour while paused".
   - Interpretation used: `cheap-pause` is `yes` on the two explicit $0 statements and the pricing page; no pause-tier
     price is published anywhere, so $0 is today's price, not a promise. The usage API's `paused_cost_micro` field
     ("what the sleeping seconds cost: `0` for a browser") implies the same field can be non-zero for a VM later.

3. **Old plan names still on a live page.**
   - https://docs.styne.ai/limits.md — "Pages per crawl job | 1,000 without a plan; 2,000 Builder; 5,000 Startup;
     10,000 Scale". The plans everywhere else are Starter / Pro / Scale.
   - Consequence: that crawl-limit row is stale on the live page (it is not a cache artifact — the same text came back
     from two independent fetches today). It does not affect any row here; flag it for the parent so a reviewer does
     not "fix" a quote that really says Builder.

4. **GB vs GiB.**
   - https://styne.ai/pricing — "4 GB", "8 GB", "16 GB", "32 GB" and a 51/111/251 "GB" disk column.
   - https://docs.styne.ai/limits.md — "4 GiB", "8 GiB", "16 GiB", "32 GiB" and 51/111/251 "GiB".
   - Interpretation used: the limits page's GiB is the technical statement (it is also the unit used for the
     resident-memory limit and the 40 GiB trial figure); the pricing page uses GB loosely. Rows say GiB and the
     feature note records the mismatch.

5. **Included hours and the price cut are dated, and the hours change is one day old.**
   - https://docs.styne.ai/changelog.md, 2026-11-26 — "**VM hours counted by size.** A plan's included VM hours are
     counted at the 4 vCPU / 8 GB size: an hour on small uses half an hour, on large two, on xlarge four."
   - https://docs.styne.ai/changelog.md, 2026-11-20 — "**Lower VM prices.** After the included hours a VM hour now costs
     $0.032 for the default size (4 vCPU, 8 GB, 11 % less), $0.064 for large (8 vCPU, 16 GB, 11 % less) and $0.128 for
     xlarge (16 vCPU, 32 GB, 36 % less); small stays $0.016."
   - Not a contradiction, but a freshness trap: any cached page from before 26 November shows the old counting rule
     (an hour on small used a whole hour). The billing page and pricing page both carry the new rule today.

## Unknowns

- Hypervisor and host: no page names Firecracker, KVM or the host type. "microVM" is the only published word.
- GPU: nothing published; no size, plan, price or API field.
- Nested virtualisation / `/dev/kvm`: nothing published.
- Kernel module loading: no page says whether a sandbox may `modprobe` or load a module. An own kernel does not
  establish that guests are allowed to, so the isolation scenario leaves that step unknown.
- Docker inside a sandbox (as opposed to a CI runner): nothing published.
- Pause-tier prices: the tiers exist (`memory`, `standby`, `archived`) and the pages say they are "metered", but no
  rate is published and the billing page says $0. What a `memory` pause will cost if that changes is unknown.
- EU availability date: "coming back later" with no date.
- Storage prices: snapshots and volumes are "$0 today" with no announced price; egress, IPs and seats are not priced
  on the pages read here.
- Whether a person's dashboard signup can run anything before adding a card: `authentication.md` describes email
  signup, `billing.md` says an account with no trial or plan answers 402, and only the agent sign-up route documents
  300 free seconds.
- Trial conversion: the pages say the trial is 7 days and card required, and that a trial that reaches 20 hours
  answers 422; they do not say whether it converts automatically to Starter at day 7.

## Supplementary: the tariff, for the parent's rate card (not a deliverable of this package)

Published units, 2026-11-26:

| Size | vCPU | Memory | Disk | Overage per hour | Counted hours per running hour |
|---|---|---|---|---|---|
| small | 2 | 4 GiB | 51 GiB | $0.016 | 0.5 |
| default | 4 | 8 GiB | 51 GiB | $0.032 | 1 |
| large | 8 | 16 GiB | 111 GiB | $0.064 | 2 |
| xlarge | 16 | 32 GiB | 251 GiB | $0.128 | 4 |

Plans: Starter $20/mo (700 VM h, 350 browser h), Pro $99 (4,000 / 2,500), Scale $499 (14,000 / 7,500). Browser hour
$0.04. Paused $0. Snapshots and volumes $0 today. Search/fetch/crawl are prepaid credit, separate from plans.

Hand-worked A/B/C against `presets` in `site/src/lib/cost.ts`, assuming one billing month of 730 hours:

- **A** (2 vCPU / 4 GiB / 10 GiB, 166⅔ h, 50 % busy, deleted each run): fits `small` (51 GiB disk). 166⅔ running
  hours × 0.5 = 83⅓ counted hours, inside Starter's 700 → overage $0; the month costs the $20 plan. No separate disk
  charge.
- **B** (4 vCPU / 8 GiB / 50 GiB, 730 h, 10 % busy): fits `default` (51 GiB disk). 730 counted hours → 30 over Starter's
  700 × $0.032 = $0.96, plus $20 = **$20.96**. Pro would be $99 (730 of 4,000), so Starter is cheaper.
- **C** (2 vCPU / 4 GiB / 20 GiB, 90 running + 640 paused): fits `small`. 90 × 0.5 = 45 counted hours, inside 700;
  paused hours $0 → **$20** (the plan fee).
- Model limits for the engine: the fee is a per-account monthly subscription with an included-hours allowance counted
  by size, not a per-machine flat price and not pure metered billing. `RateCard.monthlyPlans`-style handling or a plan
  fee plus allowance is needed; overage rates are per size, so a single hourly rate cannot represent all four shapes.
  Disk is not metered separately, so running/stopped disk rates should be 0 with a note, not unknown.

## Search coverage behind the "not-published" and "no" verdicts

- All 178 `.md` pages linked from `llms.txt` were fetched and grepped: `gpu` — 0 hits; `kvm`/`nested` — 1 hit, a JSON
  path in `fetch.md`; `docker` — `ci.md` (runner image), `images.md` and `errors.md` (image references), and
  `migrate-daytona.md` (a Daytona Dockerfile row that maps to `base`).
- `styne.ai/openapi.json` (69 paths) has no `resize`, `gpu`, `kvm`, `nested` or `class` field; `size` appears only as
  the create/size parameter.
- Pages read in full for this package: `index`, `sandboxes`, `regions`, `pause-tiers`, `images`, `billing`, `limits`,
  `exec`, `guide-templates`, `changelog`, `agent-signup`, `ci`, `authentication`, `data-retention`, plus
  `styne.ai/pricing`, `styne.ai/sandboxes`, `styne.ai/legal/subprocessors`.

---

<!-- package file: p2-state.md -->
## Styne — package 2: State and staying on

Research worker ledger for adding provider id `styne` to sandbox-guide. Repository `thelaggingway/sandbox-guide`, revision `fa86e49` (local `main` = `origin/main` at start; worktree clean). Desk research only: no Styne account, key or sandbox was created. Checked **2026-11-26** (UTC). JSON rows: `research/styne-work/p2-state.json`.

Assigned features: lifecycle, snapshots-forks, storage, idle-timers. Assigned capability IDs (18): pause-memory, warm-pool, fast-start, snapshot-files, snapshot-memory, fork-running, older-snapshot, templates, backups, files-survive-stop, volumes, shared-volume, mount-buckets, big-disk, stay-on, choose-idle, wake-on-request, schedules.

## Retrieval routes and freshness

| Route | What worked | What failed |
|---|---|---|
| TinyFish `fetch_content` (Composio MCP, via code_exec) | All 18 docs pages returned full Markdown with a fresh `last_modified` header (26 Nov 2026, 18:53–19:05 GMT). Batch of 10 URLs with `ttl: 0` returned HTTP 400; per-URL calls worked. Re-ran 10 quote checks: all found. | Batch request (10 URLs, `ttl`) → HTTP 400. `llms-full.txt` (387 KB) returned an empty `results` array; used curl for it instead. |
| Context.dev `web_scrape` (Composio MCP, via code_exec) | Markdown scrape of every page worked. Re-ran 10 quote checks: all found. | `zdr: "enabled"` → 403 `ZDR_NOT_ENABLED` (org setting); retried without it. One scrape of `changelog.md` was **stale**: it lacked the 2026-11-26 entry that the live bytes contain. |
| Direct `curl` of `https://docs.styne.ai/<page>.md` | Used for the authoritative quote check (raw bytes, `Cache-Control: no-cache`) and for `llms-full.txt`. | — |

Quote verification: `python3 .amp/in/orb-backup/styne/check_quotes.py` re-fetched all 8 cited URLs live (all HTTP 200), and matched all 22 quotes after normalising only: CRLF, Markdown escapes (`\$`→`$`, `\_`→`_`, `\*`→`*`), link syntax to link text, `*`/`` ` `` emphasis, and whitespace runs. 0 misses. An independent check of 10 key quotes through TinyFish and Context also matched on both. Because Context served a stale changelog, only the live bytes are treated as proof; every page's `last-modified` was 2026-11-26 and every page is the Styne docs page named (title and `> ` description checked).

Pages read in full: `index`, `sandboxes`, `pause-tiers`, `snapshots`, `fork`, `checkpoints`, `volumes`, `export`, `state`, `guide-templates`, `wake-triggers`, `limits`, `billing`, `changelog`, `data-retention`, `quickstart`, `api-v1`, `api-reference`, `images`, `migrate-vercel-modal`, plus a keyword sweep of `llms-full.txt` (387,141 bytes).

## Capability evidence

Each row: verdict → quote → source → confidence. "Also read" lists context that the quote alone does not prove.

### pause-memory — yes
- Quote: "A pause keeps a sandbox's state so a resume is transparent: the process, its memory and its open files are where they\nwere." — https://docs.styne.ai/pause-tiers.md — `docs-confirmed`.
- Tiers: "`memory` (the default) | The whole guest memory, kept on the node", "`standby` | A snapshot of memory and disk on the node's disk; the memory is freed", "`archived` | The standby snapshot, also sealed in object storage". Same page.
- Also read: "A sandbox with a volume is killed, not paused, at its timeout" (volumes.md), so a volume-mounted sandbox has no pause at all. "A command on a paused sandbox answers `412 sandbox_paused`: resume it first" — the process is kept, but calls need a resume.
- Not claimed: that every process keeps running while paused (processes are frozen; the docs say resume is transparent).

### warm-pool — partly
- Quote: "a create answers in about 0.27 seconds, so a pool of 8 served 24 sessions in 0.5 seconds" — https://docs.styne.ai/migrate-vercel-modal.md — `docs-confirmed`.
- Also read: sandboxes.md "a create is a pop from a pool of warmed VMs"; fork.md "restores the children on pre-spawned blank VMs". So Styne itself keeps a warmed pool, but no page exposes a pool you size, pre-fill or pay for; the migration page tells the reader to keep "The same pool over `Sandbox.create`, `Sandbox.connect` and `kill`" themselves.
- Shortfall (written): "Creates draw from a warmed pool you cannot see, size or refill; there is no pool product, so you keep your own spare sandboxes and a create answers in about 0.27 seconds."

### fast-start — yes
- Quote: "a sandbox from a snapshot starts in 0.07 to 0.26 seconds" — https://docs.styne.ai/migrate-vercel-modal.md — `docs-confirmed`.
- Also read: "a create answers in about 0.27 seconds" (same page). Both are documentation figures, not measurements in this desk study; the row says so.

### snapshot-files — yes
- Quote: "You can read the files of a saved snapshot without starting it; the sandbox it came from can be stopped or gone." — https://docs.styne.ai/snapshots.md — `docs-confirmed`.
- Also read: "run `sync` in the sandbox before you save it when you need a file to show up here", so unflushed writes are not in the browsable view (they are in a restored sandbox). 32 MiB per file/archive cap.

### snapshot-memory — yes
- Quote: "each one begins exactly where the saved sandbox was: files,\nprocesses and memory included." — https://docs.styne.ai/snapshots.md — `docs-confirmed`.
- Also read: snapshots.md lead "Save a running sandbox's memory and disk under a name and start any number of sandboxes from it." A sandbox with a volume cannot be snapshotted (`409 volume_mounted`, volumes.md).

### fork-running — partly
- Quote (the row's cited quote): "The parent keeps running after the fork" — https://docs.styne.ai/fork.md — `docs-confirmed`.
- Also read: "A fork pauses a sandbox, takes a copy-on-write snapshot of its memory and disk, and restores the children on pre-spawned blank VMs" (the pause while copying); "a storm of forks at 8 times the normal create rate completed fewer than a third of its forks on the first try" (measured on the test region on 2026-11-13) → a busy node answers `503` and must be retried; children count against concurrency; `count` defaults to 1 and no upper bound on children is stated.
- Shortfall: "Forking pauses the parent while it copies, and a busy node refuses forks with a 503 that must be retried; under a burst fewer than a third completed first time."

### older-snapshot — yes
- Quote: "`GET /snapshots` lists your saved snapshots, newest first." — https://docs.styne.ai/snapshots.md — `docs-confirmed`.
- Also read: "Any sandbox of your account can be started from that name" and `POST /sandboxes` with `{"snapshot": "golden-1"}`; any named snapshot can start a new sandbox. Going back **in place** (same sandbox id) is checkpoint/rewind: "Both belong to the reliability features of the Pro and Scale plans" (checkpoints.md) and billing's "Checkpoint and rewind | No | Yes | Yes" (Starter/Pro/Scale). The row note records the plan gate; the capability as defined ("restore any earlier snapshot") is met by starting a new sandbox from any named snapshot.

### templates — partly
- Quote: "A sandbox started from a snapshot has the size of the template the snapshot came from." — https://docs.styne.ai/guide-templates.md — `docs-confirmed`.
- Also read: four templates `small` 2 vCPU/4 GiB/51 GiB, `default` (alias `base`) 4/8/51, `large` 8/16/111, `xlarge` 16/32/251 (guide-templates.md). Your own set-up state is saved as a snapshot, not built as a new template (no Dockerfile/template-builder product on these pages; `images.md` mentions prepared container images but is out of this package).
- Shortfall: "There are four fixed templates (2 to 16 vCPU) and your own start state is a snapshot, not a new size: a snapshot keeps the size of the template it came from."

### backups — no
- Quote: "If the machine itself fails, the volume is lost: a volume is kept on one machine and is not\nbacked up." — https://docs.styne.ai/volumes.md — `docs-confirmed`.
- Also read: a keyword sweep of `llms-full.txt` finds "backup" only in this sentence, in the state-store caveat ("treat the state store as durable storage, not as a backup") and in the account-deletion note. Snapshots are taken only by `POST /sandboxes/{id}/snapshots`; no page describes a scheduled backup. Verdict `no` rests on the explicit "not backed up" plus the absence of any backup feature in the full corpus. Consistent with the existing e2b row (`no`, `inferred`).

### files-survive-stop — yes
- Quote: "it stays kept, with its files and memory, until you delete it." — https://docs.styne.ai/billing.md — `pricing-page` confidence (money page; the claim itself is behavioural, also stated on pause-tiers.md: "Keeps memory, processes, files | until you kill it").
- Also read: "While a sandbox is paused no timeout runs" (pause-tiers.md). A **timeout** that ends a sandbox loses the disk unless `onTimeout: pause` was set; `DELETE` loses it. Styne has no separate "stop" state (sandboxes.md lifecycle: Running → Paused → Ended), so the row is about pause.

### volumes — partly
- Quote: "A sandbox with a volume is killed, not paused, at its timeout: `autoPause` and `lifecycle.onTimeout: \"pause\"` answer\n`422` together with `volumeMounts`." — https://docs.styne.ai/volumes.md — `docs-confirmed`.
- Also read: "A volume is a disk that outlives sandboxes"; 1 to 100 GiB (`sizeMB` 1,024 to 102,400), up to 4 per sandbox; "You pay for the size you ask for, not for what you write. A volume cannot be resized."
- Shortfall: "A volume outlives its sandbox, but a sandbox with a volume cannot pause, snapshot, fork or rewind, so it is killed at its timeout."

### shared-volume — no
- Quote: "One sandbox at a time holds a volume." — https://docs.styne.ai/volumes.md — `docs-confirmed`.
- Also read: "A create that names a volume a running sandbox holds answers `409 volume_busy` at once." Even a `readOnly` mount "still takes the volume for this sandbox alone".

### mount-buckets — not-published
- Quote (inspected page, not proof of absence): "A volume is an ext4 disk of fixed size kept on one machine of one [region](https://docs.styne.ai/regions.md)." — https://docs.styne.ai/volumes.md — `inferred`.
- Searched: `llms-full.txt` (all pages) for "S3", "s3://", "bucket": 0 hits. "object storage" appears once, as where an `archived` pause is sealed, not as a user mount. Unanswered question: can an outside bucket or filesystem be mounted as a folder? Silence → `not-published`.

### big-disk — partly
- Quote: "| `large` | 8 | 16 GiB | 111 GiB |" — https://docs.styne.ai/limits.md — `docs-confirmed` (sizes table `large` 111 GiB, `xlarge` 251 GiB, `default`/`small` 51 GiB).
- Also read: Starter's "Largest size | 8 vCPU, 16 GB" (billing.md), so `xlarge` (251 GiB) needs Scale; `large` (111 GiB) is the most Starter and Pro can start. 100 GiB or more is available, so the capability is met, but not on the cheapest plan.
- Shortfall: "The 111 GiB and 251 GiB disks need the large and xlarge templates, which the Starter plan cannot start (8 vCPU is its largest)."

### stay-on — partly (see the dedicated section below)
- Quote: "`/refreshes` keeps the sandbox alive for at least that many more seconds and never\nshortens it, so a keep-alive loop can call it freely." — https://docs.styne.ai/pause-tiers.md — `docs-confirmed`.
- Shortfall: "No single timeout lasts a month (86,400 seconds is the most), so a busy month-long agent must call /refreshes from inside itself before each deadline, and a volume-mounted sandbox cannot pause to bridge a gap."

### choose-idle — partly
- Quote: "`\"idleTimeout\": 120` (seconds, 0 to 3600; 0 or absent means none)." — https://docs.styne.ai/pause-tiers.md — `docs-confirmed`.
- Also read: "The sandbox then sleeps once nothing has called or connected to it for that long, even if a process is running"; separately "The platform puts a running sandbox to sleep only when nothing of yours runs inside it". No value means "never" for the optional idle sleep, but the maximum you can set is one hour and the platform's own sleep is not yours to configure.
- Shortfall: "The idle sleep you set lasts at most an hour (0 turns it off), and the platform separately sleeps any sandbox where nothing of yours is running."

### wake-on-request — yes
- Quote: "We store it before we answer the sender, wake the sandbox if it sleeps, and hand the event over." — https://docs.styne.ai/wake-triggers.md — `docs-confirmed`.
- Also read: "wakes in milliseconds on the next command, file call or connection" (pause-tiers.md); `onTimeout: pause` + `autoResume`: "with `autoResume`, the next call to it wakes it". A public preview URL does **not** wake a paused sandbox: "A sandbox that is paused does not serve; it wakes when you resume it" (ports.md, seen in `llms-full.txt`).

### schedules — yes
- Quote: "Five fields (minute hour day-of-month month day-of-week), minute resolution, any IANA time zone." — https://docs.styne.ai/wake-triggers.md — `docs-confirmed`.
- Also read: a cron trigger wakes the sandbox and delivers the event as a file, command or local HTTP post; `catch_up` `one`/`skip`; limits: 100 triggers per account, 20 per sandbox; "A wake by a trigger is billed like any other resume." Events of one sandbox are handed over one at a time; a trigger belongs to one sandbox, so the sandbox must exist (a killed sandbox deletes its triggers).

## Stay-on: can a Styne sandbox run for a whole month (730 hours)?

BotPerch's case B is 730 uninterrupted running hours on one machine. Styne facts that decide it:

1. `timeout` on create is "1 to 86,400" seconds from the start (86,400 s = 24 h), "on every plan and trial" (`caps.max_sandbox_lifetime_seconds` = 86,400) — limits.md. A create without one gets 300 s; a no-plan, no-trial account is held to 3,600 s.
2. Commands do not extend it: "A running sandbox keeps running its processes and its clock from create until its timeout, whether or not a command or a request is in flight" — pause-tiers.md.
3. `/timeout` "sets a new lifetime from now"; `/refreshes` "keeps the sandbox alive for at least that many more seconds and never shortens it". Both answer 204 and are SDK calls (`sbx.set_timeout`). Neither is capped lower than 86,400 in the docs.
4. If the deadline passes, the sandbox is **killed** (`410 sandbox_timeout`) unless created with `lifecycle.onTimeout: pause` (then it pauses and can `autoResume`).

**Conclusion:** a sandbox can stay up for 730 hours only through a keep-alive loop *inside* it that calls `/refreshes` (or `/timeout`) before each deadline — the page's own phrase, "so a keep-alive loop can call it freely", says this is expected use. With `onTimeout: pause` + `autoResume` a missed deadline becomes a pause instead of a kill, and the clock stops while paused. So:
- Without a keep-alive: max 24 h running, then paused (if configured) or killed.
- With a keep-alive process (e.g. a 1-hour `/refreshes` every 30 minutes, or any interval under 86,400 s): indefinite running, billed per second the whole time. Each call must come from the reader's own loop or cron-inside-the-sandbox; Styne has no "no timeout" setting.
- A sandbox with a **volume** cannot use `onTimeout: pause` (`422`) and cannot be paused at all, so it is killed at its timeout; for a volume-backed month-long job the keep-alive loop is mandatory, not a safety net.

Verdict `partly`, figure "86,400 s; keep-alive": a busy month is possible but needs a self-refresh mechanism and there is no setting that makes a sandbox exempt from timeouts. This matches how the site treats e2b's 24 h cap (`partly`) but differs because Styne documents the refresh route.

## First-party contradictions

1. **Does a memory pause take a concurrency slot?**
   - pause-tiers.md: "A memory pause keeps its memory on the node and still counts against your concurrent sandboxes; a `standby` or `archived` sandbox does not".
   - limits.md: "Only a running sandbox counts: a sleeping one (`standby`, paused in memory or `archived`) costs nothing and takes no running slot".
   - billing.md: "`concurrent_sandboxes`; a sleeping one is not counted".
   - Interpretation used by the rows: the memory-pause slot claim is a **resident** limit, not the running-VM concurrency limit. limits.md's separate "Sandboxes resident on the nodes (running, or paused in memory)" row (ceiling "plan 100, or the entitlement's `target` when that is higher") is the consistent reading. Consequence: a memory-paused sandbox does not use a running slot, but it does use the resident ceiling and the "held in any state" ceiling; `standby`/`archived` use only the latter. Parent should decide whether to mention this in prose; the rows here do not depend on the disputed number.
2. **Paused cost.** billing.md: "A paused or sleeping VM or browser costs $0." pause-tiers.md: "A paused sandbox is metered at its tier's rate." and billing: "A paused sandbox is metered at its tier's rate." Interpretation: "metered" at a rate of $0 today (paused seconds are recorded, `paused_cost_micro`); not a positive charge. The feature row cites billing's "$0 today" storage statement separately.
3. **"Sleep" naming.** pause-tiers.md and billing.md call a paused sandbox "asleep"; limits.md lists pause tiers as "sleeping"; sandboxes.md lifecycle has only Running / Paused / Ended. Treat sleep = pause.
4. **Forked parent.** fork.md: "The parent keeps running after the fork" while "A fork pauses a sandbox … and restores the children"; the pause is momentary, not a stop.

## Unknowns / not published (searched, no answer)
- Whether a sandbox with a volume can use any pause tier indirectly (e.g. through the Daytona-compatible `stop`): volumes.md says pause/snapshot/fork/rewind answer `409 volume_mounted`; no exception is documented.
- Whether the `/refreshes` request body duration has an upper bound (the docs show `{"duration": 120}` and no maximum; limits.md lists only `timeout` 1–86,400). Assumed no bound; a reader can also just set `timeout` to 86,400 repeatedly.
- A pool you can pre-fill, size or pay for: not published.
- Scheduled automatic backups: not published; a volume is explicitly not backed up.
- S3/GCS/Azure bucket mount: not published anywhere in `llms-full.txt`.
- Retention of a paused sandbox on a paid plan beyond "until you delete it": no separate deadline is stated; the only deletion clock found is the 7-day no-credit grace (billing.md).
- `archived` pause behaviour across node failures ("sealed in object storage"): no durability or restore-time guarantee is stated.
- Figures for create and snapshot-restore times are provider documentation, not measured here.

## Notes for the parent
- `docs.json` links that fit this package: lifecycle → https://docs.styne.ai/pause-tiers, snapshots-forks → https://docs.styne.ai/snapshots, storage → https://docs.styne.ai/volumes, idle-timers → https://docs.styne.ai/wake-triggers (or pause-tiers; wake-triggers covers timers + wakes).
- The changelog has an entry dated 2026-11-26 (VM hours counted by size) that is pricing-package territory; it is why Context's cached changelog looked stale.
- If the parent writes feature prose that says Styne can "stay on", it must include the keep-alive requirement; Styne has no "never times out" switch.

---

<!-- package file: p3-access.md -->
## Styne — Access package (reaching-it, custom-domains, outbound-network, keys-secrets)

Provider id: `styne` · Docs: https://docs.styne.ai/ · Repository revision: `fa86e49` (local `main`, no changes made to shared data) · Research date: **2026-11-26** · Desk research only: no Styne account, key or sandbox was created, and nothing billable was started.

Companion file: `research/styne-work/p3-access.json` (15 capability rows, 4 feature rows, proposed RateCard `egress`).

## Retrieval routes

| Route | Used for | Result |
|---|---|---|
| TinyFish MCP `fetch_content` | `llms.txt`, `llms-full.txt`, every docs page cited below | Succeeded. Markdown rendering drops `<port>-<sandbox id>` placeholders (it strips angle-bracket text), so quotes were not taken from this route. |
| TinyFish MCP `search` | One discovery query for egress/custom-domain/static-IP material | Succeeded; returned only docs pages already in hand plus unrelated vendors, so nothing load-bearing came from it. |
| Context MCP `web_scrape` (`maxAgeMs: 0`, `formats.markdown`, `responseMode: full`) | ports, ssh, network, ai-credentials, migrate-boat, errors, tavily-google-drop-in | Succeeded. Returned 7,261 / 6,418 / 5,152 / 9,987 / 5,422 / 13,423 / 6,633 bytes — byte-for-byte the same lengths as the raw `.md` fetches, so the content served to a second fetcher is identical. |
| Direct `curl` of the raw `https://docs.styne.ai/<page>.md` with a cache-busting `?cb=` parameter | Verbatim quote source; `sitemap.xml` `lastmod`; `styne.ai/openapi.json` | Succeeded. `cf-cache-status: HIT` with a `last-modified` of the same day; the cache-buster returned the identical body. |
| Exa | Not used | A prior session reported a stale cached Styne page; the owner asked for fresh retrieval, and TinyFish plus Context covered every source, so Exa was skipped deliberately. |
| Amp native `web_search` / `read_web_page` | Not used | No need; the Composio routes worked. |

Page identity and freshness were checked with `https://docs.styne.ai/sitemap.xml` (195 URLs, each with `lastmod`). The pages this package leans on:

| Page | `lastmod` |
|---|---|
| https://docs.styne.ai/network | 2026-11-26T10:53:51Z |
| https://docs.styne.ai/ai-credentials | 2026-11-26T10:53:51Z |
| https://docs.styne.ai/billing | 2026-11-26T18:11:52Z |
| https://docs.styne.ai/limits | 2026-11-26T18:11:52Z |
| https://docs.styne.ai/migrate-boat | 2026-11-26T10:20:17Z |
| https://docs.styne.ai/migrate-daytona | 2026-11-26T10:20:17Z |
| https://docs.styne.ai/ssh | 2026-11-23T16:13:52Z |
| https://docs.styne.ai/tavily-google-drop-in | 2026-11-23T16:58:34Z |
| https://docs.styne.ai/ports | 2026-11-16T22:44:06Z |
| https://docs.styne.ai/authentication | 2026-11-16T13:56:20Z |

Quote check: every quote below was re-fetched on 2026-11-26 and matched mechanically after collapsing runs of whitespace to a single space (the only normalization; backticks, punctuation, numbers and qualifiers are unchanged). All 19 checks passed, and Context returned the same bodies independently.

Page-inventory check for absences: `https://docs.styne.ai/llms.txt` lists every page and has no environments, secrets, custom-domain, static-IP or dashboard page. Probes for `custom-domains`, `domains`, `networking`, `egress`, `static-ip`, `environments`, `secrets`, `dashboard`, `terminal` and `pricing` all answer 404 on `docs.styne.ai`. The marketing site's `https://styne.ai/pricing` and `https://styne.ai/sandboxes` were read in full and mention no egress rate, IP charge or custom domain.

---

## Capabilities

### 1. public-url — **yes** · figure "Per-port URL" · `docs-confirmed`

> "A port a server listens on inside a sandbox is reachable at `https://<port>-<sandbox id>.<domain>`."
> — https://docs.styne.ai/ports.md (checked 2026-11-26; TinyFish, Context, raw curl)

The port is opened with `POST /sandboxes/{id}/ports`; `public: true` answers `{"port":3000,"public":true,"url":"https://3000-<id>.styne.ai"}` and "Open that URL in a browser or `curl` it; no token is needed." (same page, verified). A sandbox exposes at most 32 ports, port `49983` cannot be exposed, and the choice ends with the sandbox.

Buyer catch: the URL is built from the sandbox id, so a new sandbox gets a new URL; a deployment that serves no sandbox domain omits `url` entirely.

### 2. private-url — **yes** · figure "Token by default" · `docs-confirmed`

> "A sandbox serves its ports privately: every port needs the sandbox's traffic access token (header `e2b-traffic-access-token`, or the `?token=` query for `wss://`) unless you make it public."
> — https://docs.styne.ai/ports.md (checked 2026-11-26)

The token is the create answer's `trafficAccessToken`. It is **per sandbox, not per port**: one value opens every token-gated port of that machine. No rotation, separate expiry or revocation call is documented, and a killed sandbox answers 404 "to everyone, with or without a token" (same page). That is a real difference from providers whose signed links expire per port, but it still satisfies the capability, so `yes` with the catch in the note.

### 3. ssh — **yes** · figure "ssh.styne.ai:2222" · `docs-confirmed`

> "Open a sandbox with stock OpenSSH: ssh, scp, sftp, port forwards and the Remote-SSH extension of VS Code and Cursor."
> — https://docs.styne.ai/ssh.md (checked 2026-11-26)

A front door at `ssh.styne.ai:2222` with the sandbox id as the user name, per-sandbox keys (max 16, ed25519/ecdsa/rsa ≥2048), one stable host-key fingerprint, `scp`/`sftp`, and Remote-SSH blocks for VS Code and Cursor. Keys are added through `POST /sandboxes/{id}/ssh-keys`; a key belongs to the API key that added it.

Catches worth keeping: a paused sandbox answers `409` and "a connection never resumes a sandbox by itself"; "An API restart forgets the keys of every sandbox"; a template without an OpenSSH server answers `422` (the `base` template includes one); no password login.

### 4. web-terminal — **partly** · figure "In the desktop" · `docs-confirmed` quote, judgement on the shortfall

> "`desktop` starts Xfce on a virtual screen with x11vnc and noVNC and answers a URL of port 6080 that carries the session's VNC password."
> — https://docs.styne.ai/migrate-boat.md (checked 2026-11-26)

**Shortfall:** "There is no dedicated browser terminal documented; the in-browser route is a noVNC desktop opened through the boat-compatible API, where you use a terminal inside the desktop."

What exists: a noVNC desktop in the browser (above), and a PTY over a WebSocket to a login shell through the Daytona-compatible route — "`create_pty_session` and `connect_pty_session` open a WebSocket to a login shell with a terminal" (https://docs.styne.ai/migrate-daytona.md, verified), up to 16 per sandbox, which your own client renders. Neither is a terminal page on Styne's own dashboard, and the docs index has no dashboard page. This matches the existing `boat` row ("In the desktop") for the same noVNC desktop route.

### 5. tunnel — **yes** · figure "ssh -L and -R" · `docs-confirmed`

> "`-L` makes your `localhost:8080` answer with the server on port 3000 inside the sandbox. `-R` makes the sandbox's `127.0.0.1:9000` answer with the server on your machine's port 9000, for as long as `ssh` runs"
> — https://docs.styne.ai/ssh.md (checked 2026-11-26)

Both directions work, which is more than several providers in the dataset. A `-R` listener binds to loopback unless another address is named, and neither direction makes the port public.

### 6. raw-tcp — **partly** · figure "SSH forward only" · `inferred` (from documented HTTPS/WS URLs plus documented SSH forwards)

> "Websocket upgrades work on the same URL; send the token as `?token=` for a private port."
> — https://docs.styne.ai/ports.md (checked 2026-11-26)

**Shortfall:** "A non-web service is reachable only through an SSH local or remote forward; the public per-port addresses carry HTTPS and WebSocket, and no public raw TCP or UDP address is documented."

Supporting detail: the port address is an `https://` URL, and the between-sandbox call route is HTTP — "The request (method, path, query, headers and body) goes to `http://localhost:{port}/{path}` inside the named sandbox" (https://docs.styne.ai/calls.md, verified) — with WebSocket upgrades explicitly sent to an exposed port instead. Arbitrary TCP (a database, say) is reachable through `ssh -L`/`-R`. Same shape as the existing `daytona` row.

### 7. custom-domain — **not-published** · figure "styne.ai URLs only" · `not-published`

> "The URL form is `https://<port>-<sandbox id>.styne.ai`."
> — https://docs.styne.ai/ports.md (checked 2026-11-26)

Searched: the whole docs index (195 sitemap URLs), the ports, network, billing and limits pages, the marketing pricing and sandboxes pages, and the sandbox OpenAPI (`https://styne.ai/openapi.json`, "Styne sandbox API 0.1.0", 69 paths). Nothing describes a CNAME, a domain setting, a certificate for a customer hostname or an own-proxy recipe. `<domain>` in the docs is the deployment's sandbox domain, not a customer's.

Judgement call: silence is recorded as `not-published`, per the brief, even though the absence from the generated OpenAPI would support an `inferred` `no`. The parent can promote it to `no` if it prefers; nothing in the sources is an explicit negative.

### 8. auto-https — **not-published** · figure "styne.ai cert only" · `not-published`

> "A port a server listens on inside a sandbox is reachable at `https://<port>-<sandbox id>.<domain>`."
> — https://docs.styne.ai/ports.md (checked 2026-11-26)

Preview URLs are HTTPS on Styne's own domain, so Styne holds a certificate for `styne.ai`. The capability is about *your* domain's certificate, and with no custom-domain route documented there is no published certificate process to describe. Not a documented `no`.

### 9. egress-included — **not-published** · figure "no rate or allowance" · `not-published`

> "an eviction for what the sandbox did (`endReason` `evicted`, `endDetail` naming `cpu_hog`, `fork_flood`, `io_hog` or `egress_flood`)"
> — https://docs.styne.ai/errors.md (checked 2026-11-26)

The pricing page, https://docs.styne.ai/billing.md and https://docs.styne.ai/limits.md publish per-second VM rates, pause-tier rates, browser hours, storage and search/fetch/crawl prices — and no outbound byte price or monthly allowance. The `egress_flood` eviction reason shows outbound traffic is watched, which is why this is "no tariff published", not "free". `RateCard.egress.perGiB` must be `null`.

### 10. egress-allowlist — **yes** · figure "Host and CIDR lists" · `docs-confirmed`

> "Destinations the sandbox may reach: IPv4 or IPv6 addresses, CIDR blocks (`203.0.113.0/24`) and host names (`pypi.org`)"
> — https://docs.styne.ai/network.md (checked 2026-11-26)

`allowOut`/`denyOut` on create, replaced together on a running sandbox with `PUT /sandboxes/{id}/network` (answers 204, applies at once). "The rules are enforced outside the sandbox, so nothing inside it can change them." The more specific rule wins and an allow wins a tie; a host name stands for the addresses it resolves to and follows TTL changes; `allow_internet_access: false` with `allowOut` gives an allowlist and also closes IPv6. Mail ports (25, 465, 587), mining ports, the node's management addresses and other sandboxes stay closed whatever the rules say.

### 11. no-internet — **yes** · figure "allow_internet_access" · `docs-confirmed`

> "Default `true`. `false` closes the internet; only what `network.allowOut` lists stays reachable"
> — https://docs.styne.ai/network.md (checked 2026-11-26)

With `false` and an empty `allowOut`, nothing is reachable and "a sandbox with no names has no DNS" (same page). Note for the integrator: `allow_internet_access: false` also closes IPv6, whereas denying only `0.0.0.0/0` leaves IPv6 open.

### 12. static-ip — **not-published** · figure "Not published" · `not-published`

> "Set the rules on create with `allow_internet_access` and `network`, and replace them later on a running sandbox."
> — https://docs.styne.ai/network.md (checked 2026-11-26)

The rule set is destination allow/deny only; no page, price list or API operation describes a dedicated or fixed outbound address, how many a sandbox gets, or what one costs. `POST /browsers` accepts an `egress` route (https://docs.styne.ai/errors.md documents `egress_unavailable` for it), but that is the browser product and must not be read as a sandbox egress address.

### 13. secrets-store — **yes** · figure "Variables and files" · `docs-confirmed`

> "Secrets are the variables (`envContents`, `.env` text) and secret files of the account's default environment, which a new sandbox starts with."
> — https://docs.styne.ai/migrate-boat.md (checked 2026-11-26)

Also: "Secret file contents are write-only: a read lists each file's path with empty `contents`." (same page, verified). Environments can be listed, created, updated and upgraded, and a create's `environment` picks one by name (`base` is the account default). Model-provider keys are a separate store: AI credential profiles, encrypted with AES-256-GCM and never returned — "**No call ever returns a key**" (https://docs.styne.ai/ai-credentials.md, verified).

Catch to keep in the note: the documented management routes for environments and secrets are the boat-compatible `/boat/v1` ones (the docs index has no native environments or secrets page), and the values arrive inside the sandbox as ordinary variables and files the agent can read.

### 14. hidden-secrets — **partly** · figure "Model keys only" · `docs-confirmed`

> "A sandbox calls the provider through the proxy with a placeholder key, and the real key never enters the sandbox"
> — https://docs.styne.ai/ai-credentials.md (checked 2026-11-26)

**Shortfall:** "Only keys for eight named model providers are hidden from the sandbox; any other API key goes in as an environment variable the agent can read."

Detail: the proxy covers `anthropic`, `openai`, `openrouter`, `gemini`, `deepseek`, `mistral`, `xai` and `moonshot`, each at a fixed address — "A key goes only to its own provider: the host behind `/ai-proxy/anthropic/...` is fixed to `api.anthropic.com`, whatever the request says." (verified). The placeholder is random per sandbox, only its hash is stored, it is bound to that sandbox, dies when the sandbox ends and expires 2 days after create. "Sign-in subscriptions (Claude, ChatGPT), AWS Bedrock and custom provider APIs go through environment variables you pass yourself." (verified). Same verdict and shape as the existing `boat` row ("Model keys only").

### 15. scoped-keys — **yes** · figure "Presets and scopes" · `docs-confirmed`

> "A key limited to chosen endpoints needs `compat` (shown as Drop-ins in the dashboard); without it the call answers `403 endpoint_not_allowed`."
> — https://docs.styne.ai/tavily-google-drop-in.md (checked 2026-11-26)

The key API has "`PATCH` | `/v1/keys/{key}` | Rename, change preset or scopes" and "`PUT` | `/v1/keys/{key}/scopes` | Set scopes or a preset" (https://docs.styne.ai/api-v1.md, verified, generated from the served OpenAPI). Presets seen in the docs: `read-only` for reads and `full` for changes (https://docs.styne.ai/sdks.md); scopes seen: `ai.manage`, `keys.manage`, `compat`, `account.owner`. "an API key acts as the member who created it" (https://docs.styne.ai/ai-credentials.md, verified), and an owner or admin "can cap what one member's API keys may spend in a month" (https://docs.styne.ai/organizations.md, verified).

Catch: no per-sandbox scope and no key expiry are documented, so the scoping is by action and endpoint, not by machine.

---

## Feature rows (rationale)

| Feature | Verdict | Figure | Why |
|---|---|---|---|
| reaching-it | `yes` | Token by default | Public and private per-port URLs, a real SSH front door with `-L`/`-R`, scp/sftp and VS Code. Web terminal is only a desktop or a PTY you render; raw TCP needs an SSH forward. |
| custom-domains | `not-published` | styne.ai URLs only | No documented hostname route and no certificate process for a customer domain. |
| outbound-network | `yes` | Host/CIDR rules | Allow/deny lists enforced outside the machine, a full internet-off switch, live rule replacement. No egress price, allowance or fixed address published. |
| keys-secrets | `yes` | Model keys no; others yes | Model keys are placeholders outside the sandbox; other secrets are stored and injected but readable inside; API keys are scoped. |

`keys-secrets` is `yes` at feature level even though `hidden-secrets` is `partly`: the stored-secret and scoped-key halves are complete, and the existing `boat` row uses the same combination. The note and scenario carry the "model keys only" catch.

## Proposed RateCard `egress`

```json
{
  "perGiB": null,
  "note": "Not published: the pricing page, billing page, limits page and network page give no outbound byte price or monthly allowance. An `egress_flood` eviction shows outbound traffic is watched, so do not assume it is free. Not priced by the calculator.",
  "url": "https://docs.styne.ai/billing.md"
}
```

No `included` value is proposed, because nothing about an allowance is published.

## Contradictions and tensions

1. **Public by default vs private by default.** https://docs.styne.ai/ports.md says "`public` defaults to `false`" and also "A sandbox created from an E2B SDK serves its ports publicly, as E2B does, and so does one created with `network.allowPublicTraffic: true`; a sandbox made through the Daytona API is private unless it sets `public`." Both are first-party. Model used: the *sandbox* can default to open (E2B SDK, `allowPublicTraffic`), while the *per-port flag* defaults to private; an explicit port choice wins over the sandbox default. Consequence for the row: "Token by default" with the E2B-SDK exception spelled out.
2. **Docs newer than the OpenAPI.** https://docs.styne.ai/network.md documents `PUT /sandboxes/{id}/network` ("The answer is `204` … The new rules apply to the running sandbox at once"), but `https://styne.ai/openapi.json` ("Styne sandbox API 0.1.0", 69 paths) has no `/sandboxes/{id}/network` path at all. The docs page is treated as the source of truth; the OpenAPI is used only as supporting evidence for absences, and this mismatch is recorded rather than resolved.
3. **Browser `egress` is not sandbox egress.** `POST /browsers` takes an `egress` route and errors.md documents `egress_unavailable` for it; that is a browser-session proxy route. It must not be read as a static sandbox IP or as sandbox egress pricing.
4. **Paused vs asleep.** https://docs.styne.ai/ports.md: "A sandbox that is paused does not serve; it wakes when you resume it". https://docs.styne.ai/pause-tiers.md: a sandbox the platform put to sleep "wakes in milliseconds on the next command, file call or connection". Model used: an explicitly paused sandbox's URL does not serve and needs a resume; a platform-idle-slept sandbox wakes on the next connection. The feature note says exactly that.

## Unknowns (searched, unanswered)

- **Egress price and allowance.** Absent from the pricing page, billing, limits and network pages. `perGiB` is `null`; not zero.
- **Static outbound address.** Nothing published; no API operation.
- **Custom domain and its certificate.** Nothing published; no page, price entry or API operation.
- **Browser terminal on the dashboard.** Not documented. A PTY WebSocket and a noVNC desktop are documented; the login-page JavaScript chunks fetched from `app.styne.ai` contain no terminal code, but that check is inconclusive because route bundles load after sign-in.
- **Traffic-token rotation or expiry.** The token is issued with the sandbox and no rotation is documented.
- **A native (non-boat) environments/secrets API or dashboard page.** Only the boat-compatible `/boat/v1` routes are documented.
- **Per-sandbox API-key scoping and key expiry.** Not documented; scopes are by action/endpoint, plus a per-member spend cap.

## Judgement calls

- `custom-domain` and `static-ip` are `not-published`, not `no`: the brief treats silence as unpublished, and no page states an explicit negative.
- `web-terminal` is `partly` ("In the desktop"), matching the existing `boat` row for the same desktop route, rather than `not-published`.
- `raw-tcp` is `partly` ("SSH forward only"), matching `daytona`, because SSH forwarding gives real TCP while the public addresses are HTTPS/WebSocket.
- `hidden-secrets` is `partly` ("Model keys only"), matching `boat`, because the proxy covers eight model providers and not arbitrary APIs.
- Dataset-level `checkedOn` in `capabilities.json` is untouched; this package's per-provider date is 2026-11-26, the day every cited page was re-read.
---

<!-- package file: p4-running-company.md -->
## Styne — package 4: running and company

Provider id `styne`. Repository `thelaggingway/sandbox-guide` at revision `fa86e49` (= `origin/main`).
Research date **2026-11-26** (UTC). Desk research only: no Styne account, no credentials, nothing billable run.

Scope: 26 capabilities — `dashboard-create`, `cli`, `python-sdk`, `ts-sdk`, `rest-api`, `mcp-server`, `pty`,
`file-api`, `agents-preinstalled`, `prompt-api`, `one-click-hermes`, `one-click-openclaw`, `browser`, `desktop`,
`hosts-other-agents`, `team-workspace`, `roles`, `sso`, `spend-cap`, `usage-alerts`, `audit-log`, `status-page`, `sla`,
`soc2`, `dpa`, `open-source` — plus the four feature rows `ways-to-drive`, `built-in-agents`, `teams-accounts`,
`company`, plus the company/profile fields.

Companion JSON: `research/styne-work/p4-running-company.json`.

## Verdict table

| Capability | Verdict | Figure | One-line reason |
|---|---|---|---|
| dashboard-create | partly | Browser yes, VM no | Browsers page starts a browser with a button; no page shows creating a Linux VM |
| cli | yes | Linux + macOS | Static `styne` binary, checksum-verified installer |
| python-sdk | yes | styne-sdk 0.2.2 | Own PyPI package, plus unmodified `e2b` 2.63.0 |
| ts-sdk | yes | styne-sdk 0.2.2 | Own npm package, plus unmodified E2B JS client |
| rest-api | yes | OpenAPI 3.1 | `api.styne.ai`, OpenAPI 3.1 at `/v1/openapi.json` |
| mcp-server | yes | mcp.styne.ai/mcp | Hosted Streamable HTTP server that creates and drives sandboxes |
| pty | yes | pty rows/cols | `pty` size on exec, stdin, resize, signals; WebSocket pty tickets |
| file-api | yes | — | Read/write/list/find/replace/chmod, signed URLs, tar for folders |
| agents-preinstalled | no | installed on first prompt | "The template does not ship either harness" |
| prompt-api | yes | POST …/prompt | `harness: claude\|codex`, event stream, steer, interrupt, resume |
| one-click-hermes | no | — | No Hermes template, guide or harness (inferred) |
| one-click-openclaw | no | — | No OpenClaw template, guide or harness (inferred) |
| browser | yes | Chromium over CDP | Chromium microVM per session, CDP URL, live view, takeover |
| desktop | partly | noVNC via /boat/v1 | Desktop documented only on the boat-compatible routes; VNC, not computer-use API |
| hosts-other-agents | no | SDK drop-ins only | Drop-ins for other sandbox SDKs, not Cursor/Devin/Managed Agents |
| team-workspace | yes | 5 roles, one wallet | Organisation = account with more than one member |
| roles | yes | 5 roles | owner, admin, developer, billing, viewer |
| sso | not-published | — | Email invites only; no SAML/OIDC/SCIM page anywhere |
| spend-cap | partly | Per-member monthly cap | Caps a member's keys against a shared balance, not the bill |
| usage-alerts | partly | credit emails only | Emails at zero credit and day 0/3/6; no user-set threshold |
| audit-log | yes | GET /v1/audit-log | Owner and admin read it; retention not published |
| status-page | yes | 99.949% sandbox API | Better Stack page, per-component uptime |
| sla | no | — | Terms promise no uptime level unless agreed in writing |
| soc2 | not-published | — | No report or trust centre anywhere checked |
| dpa | yes | Public DPA, SCCs | Public DPA with EU SCCs, UK addendum, sub-processors |
| open-source | partly | SDKs Apache-2.0 | SDKs published; no runtime, server or self-hosting |

Totals: **14 yes, 5 partly, 5 no, 2 not-published** — 26 rows.

## Method and quote checking

- Read `AGENTS.md`, `.agents/skills/maintaining-providers/SKILL.md` and `reference/provider-checklist.md`,
  `site/src/data/schema.ts`, the 26 capability definitions and the four feature files in `site/src/data/`, and
  `site/src/data/providers/e2b.json` for the profile shape.
- Pages were discovered from `https://docs.styne.ai/llms.txt` (every page) and `https://styne.ai/sitemap.xml`
  (17 URLs), then read with the connected **TinyFish** `fetch_content` (ttl 0) and **Context** `web_search`, and
  re-fetched by direct HTTPS GET of the same official URL for byte-level quote checking. Retrieval log below.
- Every quote in this file and in the JSON was machine-checked as a normalised substring of the page fetched
  **2026-11-26** from the cited URL: 59/59 claim quotes present, and all 40 quotes carried in the JSON capability
  rows, feature rows and company facts present, 0 misses in either check. Normalisation, and nothing else: block tags to
  newlines and HTML entities decoded for the `styne.ai`/`status.styne.ai` pages, Markdown emphasis, backtick,
  heading and blockquote marks and backslash escapes removed, whitespace runs collapsed to one space, and a space
  left before punctuation by that stripping removed again. Numbers, qualifiers and wording are unchanged.
- Freshness: every quoted page returned HTTP 200 from the cited URL on 2026-11-26 between 19:00 and 19:08 UTC.
  `docs.styne.ai/billing.md` sent `last-modified: Thu, 26 Nov 2026 18:53:18 GMT`. No cached scrape is the basis of
  any quote; the direct GET is the copy checked.
- `not-published` facts in the JSON carry a source too: the page where the fact would be published, quoted
  verbatim and tagged `not-published`, with the unanswered question written into the value. `CompanyFact.source` is
  optional in `schema.ts`, but a reader should be able to see what was inspected.
- Verdicts follow the site's rules: `yes` only where the capability as defined is supported, `partly` for a
  buyer-relevant shortfall with one plain sentence, `no` only with an explicit negative or a justified inference
  from the page where support would be documented, and `not-published` where the docs are silent.

## Capability evidence

Each entry: verdict, source URL, the verbatim quote that was re-fetched and checked, confidence, checked date.

### dashboard-create — partly

- **Figure:** `Browser yes, VM no`
- **Shortfall:** The dashboard's Browsers page starts a browser session with a button, but no published page shows creating a Linux VM from the dashboard; the quickstart creates VMs from the CLI, an SDK or curl.
- **URL:** https://docs.styne.ai/browsers.md
- **Quote:** "The Browsers page lists the account's sessions with their state, region and end time. **Start a browser** starts one"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** The dashboard is real and broad — API keys page, **Team** page, usage page, billing, a CI tab, a sandbox
  page with CPU-baseline metrics and a Region column in the lists (`docs.styne.ai/regions.md`: "The Dashboard shows
  the same in a Region column in the lists."). What is missing is a documented way to start a **sandbox** by
  clicking. `docs.styne.ai/quickstart.md` starts with `pip install e2b` / `npm install e2b` / curl / the CLI, and the
  docs index has no "create a VM" dashboard page. A browser session is a machine (Chromium in its own microVM), so
  the capability is met for browsers and not for VMs.

### cli — yes

- **Figure:** `Linux + macOS`
- **URL:** https://docs.styne.ai/cli.md
- **Quote:** "`styne` is a single static binary for Linux and macOS (amd64 and arm64)."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** One-line installer checks the release checksum; commands cover `login`, `vm create/list/get/exec/files/
  pause/resume/kill/snapshot/fork/ports/logs/history/ssh`, `snapshot`, `template`, `regions`, `mcp`, `agent-guide`
  and `browser create/list/connect/rm`. `STYNE_API_KEY` wins over the stored key so CI needs no login step.

### python-sdk — yes

- **Figure:** `styne-sdk 0.2.2`
- **URL:** https://docs.styne.ai/sdks.md
- **Quote:** "| Styne Python | `styne-sdk` (`pip install styne-sdk`, `import styne`)"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** PyPI metadata (checked 2026-11-26) gives `styne-sdk` 0.2.2, `license_expression: Apache-2.0`,
  `requires_python: >=3.11`, repository `github.com/styne-ai/styne-python`. Sync and async clients (`styne.Client`,
  `styne.AsyncClient`) per the repository README. The unmodified `e2b` Python package 2.63.0 also works
  (`docs.styne.ai/e2b-drop-in.md`).

### ts-sdk — yes

- **Figure:** `styne-sdk 0.2.2`
- **URL:** https://docs.styne.ai/sdks.md
- **Quote:** “| Styne TypeScript | `styne-sdk` (`npm install styne-sdk`, `import { Styne } from "styne-sdk"`)”
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** npm registry metadata: `styne-sdk` 0.2.2, `license: Apache-2.0`, repository
  `github.com/styne-ai/styne-js`, five versions, package created 2026-11-16. The unmodified E2B JavaScript client
  also works.

### rest-api — yes

- **Figure:** `OpenAPI 3.1`
- **URL:** https://docs.styne.ai/api-v1.md
- **Quote:** "The v1 API is served at https://api.styne.ai/v1. This reference is generated from the OpenAPI document the API itself serves at /v1/openapi.json"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Two documents exist and both load: `https://styne.ai/openapi.json` (title "Styne sandbox API", 0.1.0,
  69 paths, server `https://api.styne.ai`) and `https://api.styne.ai/v1/openapi.json` (title "Styne API",
  1.0.0-draft, "Draft 2026-11-16"). Every call takes `Authorization: Bearer`, answers snake_case JSON with
  `x-request-id` and rate-limit headers.

### mcp-server — yes

- **Figure:** `mcp.styne.ai/mcp`
- **URL:** https://docs.styne.ai/mcp.md
- **Quote:** "Endpoint: `POST /mcp` on the MCP host of your deployment, for example `https://mcp.styne.ai/mcp`."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Hosted, stateless MCP Streamable HTTP over your own key, so the same limits and bill apply. Tools:
  `sandbox_create/list/kill/pause/resume`, `run_command`, `run_code`, `file_write/read/list`, `get_preview_url`,
  `sandbox_fork`, `snapshot_create/list/delete`, `browser_create/list/close`. This is a control-plane server that
  creates sandboxes, not a tool surface inside one. `styne mcp --client NAME` prints the client entry.

### pty — yes

- **Figure:** `pty rows/cols`
- **URL:** https://docs.styne.ai/exec.md
- **Quote:** “`timeoutMs`, `stdin` (keep stdin open) and `pty` (`{"rows": 24, "cols": 80}`)”
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Later body lines send `stdin`, `closeStdin`, `signal` and `resize` (`{"rows": 40, "cols": 100}`) to the
  running process. `api-v1.md` lists `POST /v1/tickets` to "Mint a single-use WebSocket ticket (pty, live view,
  CDP)". The Daytona-compatible surface opens PTY sessions to a login shell, up to 16 per sandbox, and their output
  stays readable by a later connection (`migrate-daytona.md`).

### file-api — yes

- **Figure:** (none)
- **URL:** https://docs.styne.ai/files.md
- **Quote:** "Move files in and out of a sandbox, find and change them in place, and share a file with a link."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** SDK `files.write/read/list`, signed upload and download URLs (1 to 86,400 s, default 300), folder moves
  as a tar, and the API's `files`, `files/list`, `files/search`, `files/find`, `files/replace`, `files/permissions`
  and `files/signed-url` paths in `openapi.json`.

### agents-preinstalled — no

- **Figure:** `installed on first prompt`
- **URL:** https://docs.styne.ai/agents.md
- **Quote:** "The template does not ship either harness."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Explicit negative. The default `base` template is "Linux, root user, Python 3, Node, git, curl"
  (`guide-templates.md`), and "The first prompt of a harness installs it into `~/.agent/npm`". A buyer who wants
  Claude Code on disk before the first call has to build a snapshot themselves.

### prompt-api — yes

- **Figure:** `POST /sandboxes/{id}/prompt`
- **URL:** https://docs.styne.ai/agents.md
- **Quote:** "Run Claude Code or the Codex CLI inside a sandbox with a prompt, follow its events, steer or interrupt it, and resume the conversation later."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** `POST /sandboxes/{id}/prompt` with `harness: claude` (default) or `codex`, a prompt of 1 byte to 256 KiB,
  `permissionMode`, `cwd`, `model` and `env`; answers `202` and streams typed events (`prompt.started`,
  `harness.installing`, `agent.event`, `prompt.finished`). Steer and interrupt routes exist, conversations live in
  `~/.agent` and survive pause/resume and snapshots. **You pass your own model credentials with every prompt**
  ("Styne does not supply a model"), so the model bill stays with you. That is a hosted loop over your key, not a
  managed agent; the capability as defined ("you send a prompt, it does the work in the sandbox") is met.

### one-click-hermes — no

- **Figure:** (none)
- **URL:** https://docs.styne.ai/agents.md
- **Quote:** "| `harness` | `claude` (default) or `codex`. |"
- **Confidence:** inferred · **Checked:** 2026-11-26
- **Note:** No Hermes page in `llms.txt`, no Hermes template in `guide-templates.md` (only `small`, `default`/`base`,
  `large`, `xlarge`), no Hermes mention anywhere in `llms-full.txt`. The only documented agent harnesses are Claude
  Code and Codex. Hermes could be installed by hand like any npm/pip package, but there is no one-click or
  preinstalled route and none is documented.

### one-click-openclaw — no

- **Figure:** (none)
- **URL:** https://docs.styne.ai/agents.md
- **Quote:** "| `harness` | `claude` (default) or `codex`. |"
- **Confidence:** inferred · **Checked:** 2026-11-26
- **Note:** Same evidence as above: no OpenClaw page, template or guide, and no mention in the docs corpus. No
  gateway, onboarding or preview-URL walkthrough is published for it.

### browser — yes

- **Figure:** `Chromium over CDP`
- **URL:** https://docs.styne.ai/browsers.md
- **Quote:** "A browser session is a Chromium instance in its own microVM that you drive over the Chrome DevTools Protocol (CDP) with Playwright, Puppeteer, Stagehand or Browser Use."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Browsers are a separate product with their own template, lifecycle and meter (browser hours), same API
  key. Profiles keep one end user's logged-in browser by name; a login vault stores username, password and TOTP so a
  profile can sign in again; the dashboard has a live view with **Take control** / **Return control** human
  takeover; the Browserbase and Steel SDKs work by changing the base URL. Homepage timings (2026-11-18) claim 135 ms
  to create, connect, load a page and release, median of 100.

### desktop — partly

- **Figure:** `noVNC via /boat/v1`
- **Shortfall:** The Xfce/x11vnc/noVNC desktop is documented only under the boat-compatible routes, not in Styne's own API reference, and it is a VNC screen a person takes over rather than a computer-use API with mouse, keyboard and screenshot calls.
- **URL:** https://docs.styne.ai/migrate-boat.md
- **Quote:** "`desktop` starts Xfce on a virtual screen with x11vnc and noVNC and answers a URL of port 6080 that carries the session's VNC password."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** The sentence sits in the boat drop-in's list of compatible routes served under `/boat/v1`. The native
  surface does not carry it: `https://styne.ai/openapi.json` (69 paths) has no `desktop`, `vnc` or `display` path,
  and `api-v1.md` lists none. Sandboxes of a `desktop` template "have it at once; in another sandbox the first call
  installs it and answers `provisioning: true`". For a computer-use agent the missing piece is a programmatic
  mouse/keyboard/screenshot API — a VNC endpoint needs a client.

### hosts-other-agents — no

- **Figure:** `SDK drop-ins only`
- **URL:** https://docs.styne.ai/agents.md
- **Quote:** "Run Claude Code or the Codex CLI inside a sandbox with a prompt"
- **Confidence:** inferred · **Checked:** 2026-11-26
- **Note:** No guide, page or mention for Cursor cloud agents, Devin, Claude Managed Agents, the OpenAI Agents API,
  Amp, OpenCode, Kiro or Kimi anywhere in `llms-full.txt`. What Styne does publish is wire-compatibility with other
  **sandbox SDKs** — E2B, Daytona, Vercel Sandbox and Modal, boat, Browserbase, Steel — which lets those clients
  point at Styne. That is a migration path for code, not documentation that Styne is the machine for those
  platforms' agents.

### team-workspace — yes

- **Figure:** `5 roles, one wallet`
- **URL:** https://docs.styne.ai/organizations.md
- **Quote:** "An organization is an account with more than one member. There is no second wallet: the account's plan, hours included, keys and sandboxes belong to the account"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Members are invited by email (link valid once, 7 days), a member can belong to several accounts and
  switch in the dashboard, sandboxes belong to the account rather than the person who started them, and removing a
  member revokes every live key they created while their sandboxes keep running. There is no published per-seat
  price: the plan, included hours and keys belong to the account.

### roles — yes

- **Figure:** `5 roles`
- **URL:** https://docs.styne.ai/organizations.md
- **Quote:** "| Role | Keys | Sandboxes and logs | Billing | Members | Audit log |\n| - | - | - | - | - | - |\n| Owner | read, write | yes | yes | manage, transfer | yes |\n| Admin | read, write | yes | yes | manage (not other admins) | yes |\n| Developer | read, write | yes | no | view | no |\n| Billing | no | usage only | yes | view | no |\n| Viewer | read | read | no | view | no |"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** A genuine five-level model: viewer is read-only, developer cannot see billing, billing cannot use keys,
  admin cannot manage other admins, and one owner per account moves only by transfer. Changes to members need a
  recent sign-in with a second factor if the account has one.

### sso — not-published

- **Figure:** (none)
- **URL:** https://styne.ai/legal/subprocessors
- **Quote:** "Sign-in runs on our own servers with open-source software; no outside identity provider receives your data."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Searched and unanswered. No SAML, OIDC, SCIM, "single sign-on" or "identity provider" support appears in
  `llms-full.txt`, the organizations page, the pricing page or the legal set; the only documented way in is the
  email invite ("They get an email with a link that works once for 7 days."). The sub-processor sentence says
  Styne's own sign-in does not involve an outside IdP, which is consistent with no SSO and is why this is
  `not-published` rather than `no` — a SAML integration would still leave Styne's own session in place.

### spend-cap — partly

- **Figure:** `Per-member monthly cap`
- **Shortfall:** Caps are per member and cap use of a shared balance rather than the bill: a paid plan keeps billing overage to the card, and the `/v1/budgets` endpoint is listed with no published rule for whether a budget blocks or only records.
- **URL:** https://docs.styne.ai/organizations.md
- **Quote:** "The account balance is shared; a cap only limits how much of it one member's keys can use."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** `PUT /v1/members/{user_id}/cap {"monthly_spend_cap_micro": …}` is set by an owner or admin and "applies
  to keys the member creates afterwards: a key with no cap of its own gets the member's cap, and a higher cap is
  lowered to it." Keys the member already holds keep their own cap. Above the caps, the account itself has no
  documented hard stop: `billing.md` says a plan's included hours are followed by overage "billed to the card on your
  subscription", and `limits.md` says a plan "goes on at the hourly overage rate". `api-v1.md` also lists
  `GET/PUT /v1/budgets` and `DELETE /v1/budgets/{budget}` ("Set a budget for account, project or key") with no page
  describing whether a budget refuses work or only reports — an unknown, recorded here rather than assumed.

### usage-alerts — partly

- **Figure:** `credit emails only`
- **Shortfall:** Styne emails when credit runs out and on days 0, 3 and 6 of the grace period, but nothing lets you set your own spend threshold.
- **URL:** https://docs.styne.ai/billing.md
- **Quote:** "We email you on day 0, day 3 and day 6"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** The full sentence is about deletion warnings: "We email you on day 0, day 3 and day 6: "your data is
  deleted on `<date>` unless you add a card or plan"." At zero credit "The dashboard shows a banner and we send you
  an email." `GET /v1/run/no-credit` reports `state`, `since`, `pause_at` and `delete_at`. No alert threshold,
  percentage or recipient setting is published anywhere.

### audit-log — yes

- **Figure:** `GET /v1/audit-log`
- **URL:** https://docs.styne.ai/organizations.md
- **Quote:** "GET    /v1/audit-log                     who changed what"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Read by owner and admin only (role table). It covers member and role changes, and "the audit log records
  who started, paused or stopped each one" for sandboxes. Retention, export format and whether reads are logged are
  not published. This is an account audit log; the separate command history (`GET /sandboxes/{id}/command-history`)
  is a lifecycle log of commands, not a compliance audit trail.

### status-page — yes

- **Figure:** `99.949% sandbox API`
- **URL:** https://status.styne.ai
- **Quote:** "All services are online"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26 19:03 UTC
- **Note:** Better Stack page ("Powered by Better Stack"), "Last updated Nov 26, 2026 at 7:03pm UTC", with
  per-component uptime: Sandbox API 99.949%, Sandbox API (ash) 99.950%, Website 99.929%, Documentation 100%,
  Dashboard 99.942%, Browsers 99.997%, Sandbox create 99.920%, MCP 99.593%. The MCP figure is the lowest and the
  page gives no measurement window on its face.

### sla — no

- **Figure:** (none)
- **URL:** https://styne.ai/legal/terms
- **Quote:** "we do not promise any level of uptime unless we agree a service level in writing"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Explicit negative in §11 Disclaimers: "The Service is provided "as is" and "as available". Sandboxes can
  stop because of hardware failure, maintenance or capacity limits, and we do not promise any level of uptime unless
  we agree a service level in writing." No SLA page exists on the site (17-URL sitemap) and no service credit is
  described anywhere. A customer can negotiate one in writing; nothing public states a number.

### soc2 — not-published

- **Figure:** (none)
- **URL:** https://styne.ai/legal/dpa
- **Quote:** "Each sandbox runs in its own virtual machine with its own kernel, separated from other customers."
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Searched and unanswered. No "SOC 2", "ISO 27001", "penetration test", "trust centre" or report request
  appears in `llms-full.txt`, the docs index, the pricing page, the 17-URL sitemap, the terms, the privacy policy or
  the DPA. The DPA's Annex 2 ("Security measures") lists only per-VM isolation, revocable account-scoped API keys,
  TLS, limited staff access and per-account abuse monitoring. Absence of a report is recorded as
  `not-published`, not `no`: a young vendor may have one under NDA.

### dpa — yes

- **Figure:** `Public DPA, SCCs`
- **URL:** https://styne.ai/legal/dpa
- **Quote:** "It applies when Styne Labs processes personal data on your behalf as your processor"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** Public page, "Last updated: November 12, 2026", "This addendum is part of the Terms of Service."
  Eleven sections plus two annexes. International transfers: "the European Commission's standard contractual clauses
  (module 2 or 3) apply, with the UK addendum and Swiss changes where needed, and are incorporated by reference."
  Includes a sub-processor authorisation with an objection right and refund of unused prepaid value, breach notice
  "without undue delay", deletion rules (closing an account does not delete sandboxes by itself) and one audit a
  year on 30 days' notice at the customer's cost.

### open-source — partly

- **Figure:** `SDKs Apache-2.0`
- **Shortfall:** Only the Python and TypeScript SDKs are published, under Apache-2.0; no sandbox runtime, server or self-hosting path is published, and the CLI is a signed binary with no repository.
- **URL:** https://raw.githubusercontent.com/styne-ai/styne-python/main/LICENSE
- **Quote:** "Apache License Version 2.0, January 2004"
- **Confidence:** docs-confirmed · **Checked:** 2026-11-26
- **Note:** `github.com/styne-ai` has exactly two public repositories, `styne-python` and `styne-js`, both
  "Apache-2.0", both created 2026-11-16, two commits each, no forks and no stars. The CLI is distributed as a signed
  static binary from `styne.ai/cli` with `SHA256SUMS` and has no repository. Nothing in the docs describes
  self-hosting, BYOC or running the sandbox service on your own servers. So the "open source" half is met for client
  code only, and the "self-hostable" half is not offered.

## Feature rows

### ways-to-drive — yes

- **Figure:** `Browser in dashboard; VM by SDK/CLI`
- **Scenario:** "An SDK or the CLI creates the VM, the hosted MCP server gives an assistant the same tools, and the dashboard starts a browser but no page shows creating a VM there."
- **Note:** "REST API with OpenAPI 3.1, Python and TypeScript SDKs, a static CLI, a hosted MCP server, PTY and file APIs, SSH, and drop-in wire formats for E2B, Daytona, Vercel/Modal, boat, Browserbase and Steel. No documented dashboard path creates a Linux VM; the Browsers page starts a browser session with a button."
- **Source:** https://docs.styne.ai/browsers.md — "The Browsers page lists the account's sessions with their state, region and end time. **Start a browser** starts one" (docs-confirmed, 2026-11-26).

### built-in-agents — partly

- **Figure:** `Prompt API; 2 harnesses`
- **Scenario:** "Post a prompt with harness claude or codex and your own model key: the first turn installs the harness from npm, then events stream back and the conversation resumes later."
- **Note:** "No agent in the default image and no model supplied by Styne, so the model bill stays yours. Claude Code and Codex are the only harnesses; no Hermes, OpenClaw, Cursor, Devin or Managed Agents guide. A browser (Chromium over CDP) and a noVNC desktop are separate products."
- **Source:** https://docs.styne.ai/agents.md — "The template does not ship either harness." (docs-confirmed, 2026-11-26).

### teams-accounts — partly

- **Figure:** `Per-member caps; no SSO`
- **Scenario:** "Five people share one account and one wallet: each member's new keys can be capped monthly and the audit log shows who started what, but SSO, SCIM, a SOC 2 report and an account-level hard cap are not published."
- **Note:** "One plan, one wallet, five roles (owner, admin, developer, billing, viewer), invites by email, per-member monthly spend caps that apply to keys created afterwards, an audit log for owner and admin, emails at credit exhaustion, a public DPA with SCCs and a sub-processor list. No SSO or SCIM, no published audit retention, and `/v1/budgets` is listed without documented behaviour."
- **Source:** https://docs.styne.ai/organizations.md — "The account balance is shared; a cap only limits how much of it one member's keys can use." (docs-confirmed, 2026-11-26).

### company — partly

- **Figure:** `Styne Labs; no jurisdiction`
- **Scenario:** "Procurement gets an entity name with no jurisdiction, no SLA and no SOC 2; the DPA and sub-processor list are public and the status page is a real Better Stack page."
- **Note:** "Styne Labs, with no legal form, jurisdiction, registration or address published and no governing law in the disputes section. No founder, funding or headcount published, and no press coverage found. Public since 12–13 November 2026. Status page: Sandbox API 99.949%, MCP 99.593%, all services online on 26 November 2026. The terms promise no uptime level unless agreed in writing. Public DPA with EU SCCs and five sub-processors. SDKs Apache-2.0; runtime closed."
- **Source:** https://styne.ai/legal/terms — “These terms are an agreement between you and Styne Labs ("we", "us").” (docs-confirmed, 2026-11-26).

## Company profile

### Legal entity and jurisdiction

- **Entity: “Styne Labs”.** Terms §1: “These terms are an agreement between you and Styne Labs ("we", "us").”
  Privacy §1: “Styne Labs ("we") runs the Styne sandbox service.” The DPA, sub-processor page and every footer say
  the same ("© 2026 Styne Labs", https://styne.ai/legal/dpa). · docs-confirmed, 2026-11-26.
- **Jurisdiction: not published.** No legal form (Inc., Ltd, GmbH, B.V.), company number, registered address or VAT
  number appears on the site, terms, privacy policy or DPA. §15 Disputes contains no governing-law or forum clause —
  it opens "Talk to us first." and goes on to a 30-day informal-resolution window, a no-class-actions clause and a
  one-year claim limit, with no law named. · docs-confirmed for the absence of the clause (the text was read in
  full); `not-published` for the jurisdiction. Searched: terms, privacy, DPA, sub-processors, acceptable use,
  pricing, homepage, sitemap, docs index.
- Contact for notices: "Styne Labs, support@styne.ai" (§17).

### Founding, founders, funding, headcount

- **Founded: not published.** No about, company or team page exists (`https://styne.ai/sitemap.xml` lists 17 URLs,
  none of them an about page). The earliest dated public material is the legal set, all "Last updated: November 12,
  2026", and documentation published 2026-11-13 (`changelog.md`). GitHub's record of the organisation
  `styne-ai` gives `created_at: 2026-11-13T16:23:06Z` (GitHub API, `third-party` platform record). That is a
  product-launch date, not a founding date, and I have not inferred one.
- **Founders: not published.** No founder, CEO or team member is named on the site, in the docs, in the legal pages
  or on GitHub — "This organization has no public members." (https://github.com/styne-ai, `third-party`).
- **Funding: not published.** No round, investor, accelerator or valuation appears on the homepage, pricing page,
  terms, privacy policy or DPA. The site's complete navigation — the footer on every page — lists only Products,
  Pricing, Developers, Compare and Legal sections, with no about, company or investors page:
  "Products Sandboxes Browsers Search CI runners Pricing Developers Docs Browser quickstart Changelog llms.txt Status
  Discord X SDK: GitHub SDK: npm SDK: PyPI Compare Styne vs E2B Styne vs Daytona Styne vs Modal Styne vs Vercel
  Sandbox Styne vs Browserbase Legal Terms Privacy Acceptable use Sub-processors DPA"
  (https://styne.ai/legal/terms, quote checked 2026-11-26, confidence `not-published`). TinyFish search, Context
  `web_search`, Context `company_search` and HN Algolia found no press coverage either.
- **Headcount: not published.** No team, about or careers page exists: the same footer and the 17-URL sitemap list
  only product, pricing, docs, compare and legal pages, and the GitHub organisation "has no public members."
  (https://github.com/styne-ai, `third-party`, checked 2026-11-26). No LinkedIn company page was found.

### Location and hosting

- **No registered address published.** Where the product runs is published instead:
  - "A VM or browser runs in US East (Ashburn). Amsterdam (EU) is coming back later" (https://docs.styne.ai/regions.md,
    docs-confirmed). "Machines in Northern Virginia."
  - Sub-processors (https://styne.ai/legal/subprocessors, docs-confirmed): **Hostkey** — "Bare-metal servers that run
    sandboxes", data "Everything inside sandboxes while they run or are paused", location **Netherlands**;
    **Hetzner** — "Servers for the website, dashboard, control plane and account database", location **Germany,
    United States**; **Stripe** (payments, US and worldwide); **Resend** (email, US); **Cloudflare** (DNS/TLS,
    worldwide). Sign-in "runs on our own servers with open-source software".
- Consequence for a buyer: the only live compute region is US East, `ams` is accepted but served in Ashburn, and the
  sub-processor page places the sandbox host in the Netherlands while the region page places the machines in
  Northern Virginia — see Contradictions.

### Launch history

| Date | What | Source |
|---|---|---|
| 2026-11-12 | Terms, privacy, DPA, acceptable use and sub-processor pages "Last updated" | styne.ai legal set |
| 2026-11-13 | "Documentation published", sandbox API at api.styne.ai, export/import, error `origin` field | docs.styne.ai/changelog.md |
| 2026-11-16 | First SDK releases (`styne-sdk` 0.1.0 on PyPI at 18:25 UTC, npm package created 19:55 UTC); both GitHub repos created; OpenAPI v1 marked "Draft 2026-11-16" | PyPI, npm, GitHub, api.styne.ai/v1/openapi.json |
| 2026-11-18 | Homepage "Measured timings" (51 ms snapshot restore, 19 ms resume, 135 ms browser) measured from us-east-1 | styne.ai |
| 2026-11-20 | VM overage prices cut ($0.032 default, 11% lower) | docs.styne.ai/changelog.md |
| 2026-11-23 | Search, fetch and crawl API; Tavily and Google Custom Search drop-ins | docs.styne.ai/changelog.md |
| 2026-11-26 | Plan VM hours counted at the 4 vCPU / 8 GB size | docs.styne.ai/changelog.md |

The changelog's earliest entry is 2026-11-13, so **nothing in the public record predates 12 November 2026**. No
announcement, launch post or press article was found, and I have not invented one.

### Open source and licences

- `github.com/styne-ai` publishes exactly two repositories, `styne-python` and `styne-js`, both "Apache-2.0"
  (GitHub API `license.spdx_id`), both created 2026-11-16, two commits each, no public members. The LICENSE files
  read "Apache License Version 2.0, January 2004" (raw.githubusercontent.com, docs-confirmed).
- PyPI `styne-sdk` 0.2.2 declares `license_expression: Apache-2.0`; npm `styne-sdk` 0.2.2 declares
  `license: Apache-2.0`.
- The CLI is a signed static binary installed from `styne.ai/install` with `SHA256SUMS` and no repository.
- No runtime, control-plane, scheduler or self-hosting code is published, and no page describes BYOC or running the
  service yourself.

### Status, SLA and compliance

- **Status page:** https://status.styne.ai, Better Stack, per-component uptime, all online at 2026-11-26 19:03 UTC
  (Sandbox API 99.949%, MCP 99.593%). · docs-confirmed.
- **SLA:** none. "we do not promise any level of uptime unless we agree a service level in writing" (terms §11). No
  SLA page, target or credit. · docs-confirmed.
- **SOC 2 / ISO:** nothing published. · `not-published`.
- **DPA:** public, part of the terms, EU SCCs module 2 or 3 with UK addendum and Swiss changes, sub-processor list
  with objection and refund, breach notice, annual audit right. · docs-confirmed.
- **HIPAA / regulated data:** the terms say "Do not put health, payment-card or other specially regulated data into
  the Service unless we have agreed to it in writing." (terms §7).

## Contradictions (both sides kept)

1. **Where sandboxes run.** `docs.styne.ai/regions.md`: "A VM or browser runs in US East (Ashburn). Amsterdam (EU)
   is coming back later", and the table row for `ash`: "Machines in Northern Virginia." `styne.ai/legal/subprocessors`:
   Hostkey, "Bare-metal servers that run sandboxes", location **Netherlands**. Both are first-party and both were re-read on 2026-11-26. Interpretation: the region page
   describes where the VM executes and the sub-processor page names the supplier's registered location; a buyer
   cannot tell from these pages whether sandbox data at rest or in memory sits in the US or the EU. Consequence:
   treat residency as unresolved, and note that `ams` is served in Ashburn anyway.
2. **Whether command history is kept.** `styne.ai/legal/privacy` (12 Nov 2026): "We do not keep a history of the
   commands you run in a sandbox or of their output: output is streamed to you and not stored. If we add a command
   history, this page will name it and its retention before it starts." `docs.styne.ai/exec.md` (published 13 Nov
   2026): "Every command run in a sandbox is kept, whether it came through `exec`, `POST /sandboxes/{id}/commands` or
   the E2B process API: its id (`commandId`), `pid`, `argv`, `cwd`, start and end time, status (`running`, `exited` or
   `failed`), exit code, signal and the last 1 MiB of its stdout and of its stderr." It also says "A sandbox keeps its
   last 1000 commands" and the history goes when the sandbox is deleted; `data-retention.md` adds a `zero_data_retention` switch to turn it off. Interpretation used here: a
   command history **is** kept by default and the privacy page has not caught up; the `zero_data_retention` switch
   is the way out. Consequence: a privacy or procurement answer that relies on the policy sentence is wrong today.
3. **Desktop surface.** Documented on the boat drop-in page with a port-6080 noVNC URL, absent from Styne's own
   OpenAPI (69 paths) and from `api-v1.md`. Not two conflicting statements, but the same product described in one
   place and missing in the other; recorded as a documentation gap behind the `partly` verdict.

## Unknowns (searched, not answered)

| Question | Where searched | Result |
|---|---|---|
| Legal jurisdiction, company number, address | terms, privacy, DPA, sub-processors, acceptable use, pricing, homepage, sitemap, docs index | Not published; no governing-law clause in §15 |
| Founders, CEO, team | site, docs, legal pages, GitHub org, web search | Not published; GitHub org has no public members |
| Funding, investors, valuation | TinyFish search, Context web_search, Context company_search, HN Algolia | Nothing found |
| Headcount | same | Nothing found |
| SOC 2 / ISO 27001 / pentest report | docs corpus, legal set, sitemap, pricing | Not published |
| SSO / SAML / OIDC / SCIM | docs corpus, organizations page, pricing, legal set | Not published; email invite only |
| Audit-log retention and export | organizations page, api-v1, data-retention | Not published |
| Whether `/v1/budgets` blocks spending or only records it | api-v1 (endpoint only), billing, organizations | Not published |
| Whether a dashboard button creates a Linux VM | docs corpus, quickstart, regions, sandboxes | Not published (browser creation is documented) |
| EU region date | regions.md | "Coming back later", no date |
| Per-seat pricing | billing, pricing | No seat price published; plan belongs to the account |

## Retrieval log

**Succeeded — TinyFish MCP (`fetch_content`, ttl 0):** `docs.styne.ai/llms.txt`, `docs.styne.ai/changelog.md`,
`styne.ai/`, `styne.ai/terms` (redirects to `/legal/terms`), `styne.ai/privacy`, `styne.ai/sitemap.xml`,
`styne.ai/compare`, `github.com/styne-ai/styne-python`, `github.com/styne-ai`. Clean Markdown, page identity
confirmed by `final_url` and title.

**Succeeded — TinyFish MCP (`search`):** three queries. It found the vendor's own pages and unrelated noise
(a fan wiki, an E2B funding story, a vocabulary file); one query returned 0 results. It also surfaced a **stale page
name**: a snippet cited `docs.styne.ai/api/account.md`, which today serves the Introduction page — a reminder that
the index lags the docs. No claim here rests on a search snippet.

**Succeeded — Context MCP:** `web_search` (three queries; only the vendor's own pages returned, or nothing) and
`company_search` for Styne (no results). No Context scrape was needed because TinyFish and direct GET covered every
page.

**Succeeded — direct HTTPS GET (curl) for quote checking:** every quoted page — 13 `docs.styne.ai/*.md` pages (of the
26 fetched from the `llms.txt` set), the seven `styne.ai` pages (terms, privacy, DPA, sub-processors, acceptable use,
pricing, sandboxes), `status.styne.ai`, `styne.ai/openapi.json`, `api.styne.ai/v1/openapi.json`, the GitHub REST API
(`/orgs/styne-ai`, `/orgs/styne-ai/repos`), both raw `LICENSE` files, the PyPI and npm registry JSON, and HN Algolia
search. Every quoted URL returned HTTP 200 on 2026-11-26; the copies on disk under
`.amp/in/orb-backup/styne/` are what the 59/59 quote check ran against.

**Failed or unusable:** nothing failed outright. TinyFish search and Context search/company_search produced no
third-party coverage of the company, which is itself a finding rather than a tool failure. No Exa, Parallel,
Firecrawl or Amp-native `web_search`/`read_web_page` call was made, so none is claimed.

**Not done, by design:** no account created, no API key used, no sandbox or browser started, no billable call to
`api.styne.ai` beyond reading its public OpenAPI document. Documentation claims are not measured behaviour; the
timings on the homepage are the vendor's own measurement, quoted as theirs.

---

<!-- package file: p5-browser-search.md -->
## Styne: cloud browsers and the search/fetch/crawl API

Research package 5. Desk research only: no accounts, no credentials used against Styne, Browserbase, Steel or any other product, nothing billable started. Base revision `fa86e49` (origin/main). Checked 2026-11-26 (Asia/Kolkata).

Scope: does StyneBrowser (Chromium in its own microVM over CDP, profiles, login vault, sleep/wake, fork, checkpoint/rewind, live view, human takeover, step verification, captcha modes, extensions, Browserbase/Steel-compatible APIs) and the search/fetch/crawl API genuinely differentiate Styne, and therefore how should BotPerch cover them?

**This note covers the browser and search products only.** Sandbox pricing ($20/$99/$499 plans, per-VM hour rates) is package 4's job. Numbers are repeated here only where the browser product cannot be described without them.

## Bottom line

- **Browser: yes, it differentiates Styne, but narrowly and for a specific buyer.** Styne is the only dedicated agent-browser service I found that keeps the *whole running browser in memory* while it sleeps at $0, forks a logged-in browser *mid-run*, and combines that with a live view, human takeover, a write-only login vault with TOTP, step verification and checkpoint/rewind. Every other browser vendor I checked covers one or two of those pieces (Kernel's standby is the nearest on idle cost; Browserbase's live view is the nearest on handoff). Styne is also cheaper per extra hour ($0.04) than every per-hour competitor I priced ($0.06–$0.24), and it is the only one of these that runs its own sandboxes on the same key. But it has one region (Ashburn; Amsterdam is "coming back later"), no free tier without a card, no proxy product, and no fork/checkpoint on its $20 plan.
- **Search/fetch/crawl: no, it does not differentiate Styne.** It is a competent, cheap Tavily/Brave-style API that works as a drop-in for Tavily and Google Custom Search code. At its honest rate ($1/$2/$5 per 1,000 *blocks of 10 results*) it is competitive with Brave ($5), Exa ($4–$7) and Tavily ($8–$16 equivalent), and its fetch ($1/1,000 pages) matches Exa's contents and beats Firecrawl's search credit cost. But it is not a reason to pick Styne, and it sits outside BotPerch's comparison scope. The pricing page's "$0.001 / 1,000 searches" headline contradicts Styne's own billing docs as written; the note does not resolve which unit is billed and should not.
- **BotPerch recommendation:** add Styne to the `browser-agents` use case (a `preinstalled`-tier route and a pick) and to the provider profile (`strengths` lines). On the `browser` capability, add a **`partly`** row: Styne's browser is a hosted product on its own meter, not a browser in your machine, and a bare `yes` would change what that column means. Record the search-price contradiction as a `Caveat`. **No schema change is needed.** Do not put the search API into any comparison table; at most one sentence in the use case's `outside` field, if the owner wants it.

## Retrieval routes

| Route | Used for | Outcome |
|---|---|---|
| TinyFish MCP `search` | Finding the right pages (Kernel detailed pricing, Exa/Brave/Tavily price pages, Steel and Hyperbrowser docs) | Worked |
| TinyFish MCP `fetch_content` (fresh, `ttl: 0`) | First pass over every Styne page and every competitor page | Worked for all 35 URLs in the first pass. Kernel's `/pricing/detailed` returned 404 (`page_not_found`); the right page is `kernel.sh/docs/info/pricing` |
| Context.dev MCP `web_scrape` (fresh, `maxAgeMs: 0`, `responseMode: "full"`) | Second, independent capture of every quote below | Worked; every quote cited was machine-checked (`includes()`) against the Context.dev markdown and found. Two first attempts failed for a retrieval reason, not a route reason: `responseMode: "compact"` clips to 4,000 characters (missed quotes on long pages), and the result is at `r.markdown.data`, not `r.data.markdown` |
| Shell `curl` | Checking whether Styne's docs disclose whose index it uses | Worked; no disclosure found |

Cache metadata: every Context.dev response used below reported `cache_metadata.status = "miss"`, i.e. a live fetch, not a cached copy. Styne's own pages carry no "last updated" date; the pricing page names its source as "Styne's pricing configuration, 2026-11-20", and the browser page's vendor comparison is "as of 2026-11-15". Those are Styne's dates, not our reading dates.

No Amp native web tool was needed. battleships.dev was not used for this note, so no Boat-team disclosure is required.

## 1. What StyneBrowser does and costs

### Plan limits (pricing page and billing docs agree)

| | Starter | Pro | Scale |
|---|---|---|---|
| Price | $20 a month | $99 a month | $499 a month |
| Free trial | 7 days, card required; trial capped at 2 browsers, 20 browser hours | none | none |
| Browser hours included | 350 | 2,500 | 7,500 |
| Browsers at once | 25 | 100 | 300 |
| New browsers per minute/hour/day | 30 / 300 / 1,000 | 60 / 1,000 / 5,000 | 100 / 2,000 / 10,000 |
| Extra browser hour | $0.04 | $0.04 | $0.04 |
| Sleeping browser | $0 | $0 | $0 |
| Checkpoint and rewind | no | yes | yes |
| Saved snapshots (a profile counts as one) | 20 | 100 | 300 |

A browser session's `timeout` is 15 minutes by default and may be set from 1 to 86,400 seconds.

Quotes (all found in the live pages on 2026-11-26):

- https://styne.ai/pricing — "700 VM hours and 350 browser hours included"; "4,000 VM hours and 2,500 browser hours included"; "14,000 VM hours and 7,500 browser hours included"; "200 VMs and 25 browsers at once"; "1,000 VMs and 100 browsers at once"; "2,000 VMs and 300 browsers at once"; "Checkpoint before risky actions"; "Then $0.032 per VM hour (4 vCPU default; small 2 vCPU $0.016, large $0.064, xlarge $0.128) and $0.04 per browser hour"; "Each extra browser hour"; "A sleeping browser or paused VM"; "Source: Styne's pricing configuration, 2026-11-20." — tag `pricing-page`
- https://docs.styne.ai/billing — "| VMs and browsers running at once | Trial 3 VMs and 2 browsers; Starter 200 and 25; Pro 1,000 and 100; Scale 2,000 and 300." (limits page, same wording); "1 to 86,400 seconds on every plan and trial"; "It is billed to the card on your subscription. A paused or sleeping VM or browser costs $0."; "The 7-day trial of the Starter plan has smaller limits: 3 VMs and 2 browsers at once, 150 new a day, 20 VM hours and 20 browser hours, 10 saved snapshots." — tag `docs-confirmed`
- https://docs.styne.ai/browsers — "Every field is optional: `timeout` in seconds (15 minutes by default)"; "A browser session is a Chromium instance in its own microVM that you drive over the Chrome DevTools Protocol (CDP) with Playwright, Puppeteer, Stagehand or Browser Use." — tag `docs-confirmed`

### Features, each with its source

| Feature | What Styne documents | Quote | Tag |
|---|---|---|---|
| Chromium over CDP | One browser per microVM; connect with Playwright, Puppeteer, Stagehand or Browser Use | "A browser session is a Chromium instance in its own microVM that you drive over the Chrome DevTools Protocol (CDP) with Playwright, Puppeteer, Stagehand or Browser Use." — https://docs.styne.ai/browsers | `docs-confirmed` |
| Whole-browser state | Keeps cookies, local and session storage, open tabs and the page in memory | "The browser keeps everything a person's browser would: **cookies, local and session storage, open tabs and the page in memory**." — https://styne.ai/browsers | `pricing-page` (marketing page; the docs' sleep section repeats it: "Everything in the browser comes along: open tabs, cookies, local and session storage, and the state of each page's scripts.") |
| Sleep at $0, wake on the same tab | A sleeping browser accrues no seconds; resume gives a fresh `cdp_url` | "With no client connected and nothing running, the browser **sleeps at $0 an hour**." — https://styne.ai/browsers; "The browser sleeps with its memory and leaves the running count." — https://docs.styne.ai/browsers | `pricing-page` + `docs-confirmed` |
| Fork a logged-in browser | Copies the running browser, memory and all | "**Forks with the login.**" — https://styne.ai/browsers; "Three copies of the running browser, each logged in wherever the original was." — https://docs.styne.ai/browsers | `docs-confirmed` |
| Checkpoint / rewind | Pro and Scale only; a browser takes sandbox calls because it is a sandbox | "A browser session is a sandbox with the same id, so the same calls take a browser id." — https://docs.styne.ai/checkpoints; "The browser goes back to that step and keeps its id." — https://docs.styne.ai/browsers | `docs-confirmed` |
| Live view | Screen, console and network in the dashboard | "Each session has a **live view in the dashboard**" — https://styne.ai/browsers | `pricing-page` |
| Human takeover | A person takes control for a login, captcha or payment step, then hands back; five-minute hold, audit row | "A person can take over a running browser, for a login, a captcha or a payment step, and hand it back to the agent." — https://docs.styne.ai/browsers | `docs-confirmed` |
| Step verification | Checks a step on the live page; keeps screenshot, HTML and resources for seven days | "An agent can say what it just did and what must be true on the page afterwards. The check runs inside the browser on the live page, so a step is `verified` only when the page shows it" — https://docs.styne.ai/browsers | `docs-confirmed` |
| Captcha | Click by default; `solve` for reCAPTCHA v2/v3, hCaptcha, Turnstile, GeeTest v3 and image-text; at most 10 attempts per session | "`click` (default)"; "`solve`"; "at most 10 attempts per session" — https://docs.styne.ai/browsers | `docs-confirmed` |
| Extensions | Upload unpacked extensions (50 MB zip), load up to 10 per session | "A zip is at most 50 MB and unpacks to at most 200 MB in at most 5,000 entries" — https://docs.styne.ai/browsers | `docs-confirmed` |
| Profiles (per end user) | Saved signed-in browser per end user; hosted sign-in links | "A profile is one end user's logged-in browser, kept by name." — https://docs.styne.ai/profiles | `docs-confirmed` |
| Login vault with TOTP | Write-only secret store; Styne types credentials and computes TOTP in an isolated copy; rate-limited | "The vault is write-only."; "Two-step codes are computed by Styne at the moment the code field shows, so the TOTP secret never enters the browser, and a code is never sent twice." — https://docs.styne.ai/vault | `docs-confirmed` |
| Browserbase / Steel drop-in | Same key, different base URL | "https://api.styne.ai/browserbase"; "https://api.styne.ai/steel/v1" — https://styne.ai/browsers | `pricing-page` (also in docs) |
| Region | Ashburn only; `ams` accepted but served in Ashburn | "`ash` (US East, Ashburn; the default). `ams` is accepted and served in Ashburn for now" — https://docs.styne.ai/browsers; "Europe (Amsterdam) \| `ams` \| Coming back later. Accepted, served in Ashburn." — https://docs.styne.ai/regions | `docs-confirmed` |
| Proxy | No proxy product; `egress`, `country` fields on a start | "`proxy` (send `egress` and `country`)" is refused — https://docs.styne.ai/browsers | `docs-confirmed` (no proxy bandwidth line anywhere on the pricing page — `not-published` for proxy pricing) |

### Vendor-measured claims: record as Styne's own measurements, not verified

| Claim | Styne's wording | Where |
|---|---|---|
| Browser create, connect, load and release | "**135ms** to create a browser, connect, load a page and release it (median of 100)." and "Our own measurement, 2026-11-18, from a client in us-east-1 against our ash region. Median of repeated runs, all successful." | https://styne.ai/ |
| Browser create alone | "Median 29 ms to create a browser (our own measurement, client on the same node, n=200, 0 errors, 2026-11-15)." | https://styne.ai/compare/browserbase |
| Session persistence | "30 of 30 sessions kept their state; reconnect 32 ms" — "from an AWS c7i client in us-east-1 to the US East region, 2026-11-18" | https://styne.ai/compare/browserbase |
| Browserbase comparison timing | "Median 845 ms for create, connect, first page load and close on the public Browser Arena board" | https://styne.ai/compare/browserbase — **not comparable to Styne's 29 ms create-only figure**; Styne says so itself ("This is the whole round trip, not create alone") |

These are vendor self-measurements on the vendor's own client and regions. BotPerch must not repeat them as results, and if it mentions them at all must say they are Styne's own numbers.

### The search, fetch and crawl API

| Item | Styne's statement | Source | Tag |
|---|---|---|---|
| Search price | "`fast` $1," and "$5 per 1,000 blocks" (one table cell, quoted in parts); "Per started block of 10 results"; "Eleven to twenty results are two blocks." | https://docs.styne.ai/billing ; https://docs.styne.ai/search | `docs-confirmed` |
| Marketing headline | "**$0.001** / 1,000 searches in fast mode"; "**$0.002** / 1,000 searches in auto mode"; "**$0.005** / 1,000 searches in deep mode"; each captioned "Per started block of 10 results." | https://styne.ai/pricing ; https://styne.ai/search | `pricing-page` |
| FAQ on the same pricing page | "Search is billed per started block of 10 results: $1 per 1,000 searches in fast mode, $2 in auto and $5 in deep." | https://styne.ai/pricing | `pricing-page` |
| Fetch | "$1 per 1,000 pages ($0.001 a page), charged for each page returned with text." | https://docs.styne.ai/fetch | `docs-confirmed` |
| Crawl | "$1 per 1,000 pages ($0.001 a page), charged for each page fetched." | https://docs.styne.ai/crawl | `docs-confirmed` |
| Billing | Prepaid credit, never expires; not part of the plans; no credit answers 402 | "They are not part of the plans above, and an account with no credit gets `402`." | https://docs.styne.ai/billing | `docs-confirmed` |
| Free of charge | Errors, timeouts, empty answers, blocked pages and duplicates | "Errors, timeouts, empty answers, blocked pages and duplicate pages are free." | https://docs.styne.ai/billing | `docs-confirmed` |
| Limits | Up to 20 results; fetch 1–100 URLs; crawl 1,000 pages without a plan, up to 10,000 on Scale, depth 10, 6 hours | "A search returns up to 20 results"; "Pages per job \| 1,000 without a plan; 2,000 Builder; 5,000 Startup; 10,000 Scale" | https://styne.ai/search ; https://docs.styne.ai/crawl | `docs-confirmed` |
| Rate limits | 600 requests a minute, 10 in flight; 1,200 / 20 once the account has credit | "| Requests per minute, per account | 600; 1,200 once the account has added credit; plans have their own |" | https://docs.styne.ai/search | `docs-confirmed` |
| Drop-in endpoints | Tavily and Google Custom Search request/response shapes | "Point your client at https://api.styne.ai/compat/tavily/search or https://api.styne.ai/compat/customsearch/v1 and pass your Styne key." | https://styne.ai/search | `pricing-page` |
| Tavily mapping | `advanced` → auto (the $2 mode); basic/fast/ultra-fast → fast | "`search_depth` picks the mode: `basic`, `fast`, `ultra-fast` or none run `fast`; `advanced` runs `auto`." | https://docs.styne.ai/tavily-google-drop-in | `docs-confirmed` |
| "Own index" claim | Marketing: "on our own index" | "on our own index" | https://styne.ai/search | `pricing-page` — **conflicts with the docs**: results carry "`results[].source` \| Where the result comes from, for example `web:brave`" (https://docs.styne.ai/search, `docs-confirmed`). Styne does not say whose index it is; its docs never explain `web:brave`. Treat "own index" as unverified and do not repeat it. |

### The price contradiction, stated precisely

- **Styne says two things.** On styne.ai/pricing and styne.ai/search the headline is "$0.001 / 1,000 searches in fast mode" ($0.002 auto, $0.005 deep), with the small print "Per started block of 10 results." On the same pricing page, the FAQ says "$1 per 1,000 searches in fast mode, $2 in auto and $5 in deep", and the llms.txt index says "Search is $1 / $2 / $5 per 1,000 searches (fast / auto / deep, per started block of 10 results)". The billing docs and the search docs give a table of "$1 / $2 / $5 per 1,000 blocks".
- **What the two statements say, read literally.** "**$0.001** / 1,000 searches in fast mode", at face value, is $0.001 per 1,000 searches — a millionth of a cent each. "`fast` $1 … per 1,000 blocks", with a block being 10 results, is $1 per 1,000 searches of up to 10 results — $0.10 each. As written the two differ by 1,000×. Read with the headline's caption ("Per started block of 10 results"), the $0.001 is most likely the price of one block and the two agree. Which unit Styne actually bills cannot be settled from its pages, so this note states no corrected unit and no ratio. It records only that the headline and the docs disagree as written, that the caption suggests the headline prices a block rather than a thousand searches, and that the docs, the search docs, the pricing page's own FAQ and llms.txt all give $1 / $2 / $5 per 1,000 blocks.
- **What BotPerch should use.** Any search figure must come from the docs: $1, $2 and $5 per 1,000 searches, each returning up to 10 results. If BotPerch ever quotes the search product, it should say the pricing page's headline reads "$0.001 / 1,000 searches in fast mode" while its own FAQ and the billing docs give $1, $2 and $5 per 1,000 searches, and that the two disagree as written.

## 2. Browser comparison with the dedicated agent-browser services

All competitor prices below were re-read live on 2026-11-26 from the vendor's own page and machine-checked. Per-hour figures are list prices for the regular/headless browser size unless stated.

| | **Styne** | **Browserbase** | **Steel** | **Hyperbrowser** | **Kernel** | **Browser Use Cloud** | **Browserless** |
|---|---|---|---|---|---|---|---|
| Entry paid plan | $20: 350 browser h, 25 at once | $20: 100 browser h, 25 at once | $0 Launch + usage, $30 credit once, 10 at once | $30: 30,000 credits = 30 h, 25 at once | $0 + usage, $5 credit, 5 at once | $0 pay-as-you-go, 10 at once | $25 (annual): 165 h, 10 at once |
| Free tier | 7-day trial, card required | $0: 1 h, 3 at once, 15-min sessions | $30 credit, no card | $0: 5,000 credits, 1 at once | $5 credit a month | $1 one-time credit | $0: 8 h, 2 at once, 2-min sessions |
| Price per extra browser hour | **$0.04** | $0.12 (Developer), $0.10 (Startup) | $0.10 (Launch), $0.08 (Scale) | $0.10 | $0.06 per 1 GB headless, $0.48 headful (GB-second) | **$0.06** | ~$0.20–0.24 per hour (unit overage) |
| Metering | Per second while awake | Per minute, 1-minute minimum | Per second | Per hour | Per GB-second while active | Per minute, 1-minute minimum | Units |
| Idle cost | **$0** (sleeps in memory) | Keep-alive on paid plans only; still billed until released | Billed until released | Billed while the session runs | **$0** in standby | Billed by the minute | Persisted sessions 1–90 days |
| Longest session | 24 h (1–86,400 s) | 15 min on free; plan timeout | 15 min / 1 h / 24 h | Not stated on pricing page (`not-published`) | Extended timeouts | Not stated on pricing page (`not-published`) | 2 / 15 / 30 / 60 min |
| Max concurrency | 25 / 100 / 300 | 3 / 25 / 100 / 250+ | 10 / 100 / 1,000+ | 1 / 25 / 100 / 1,000+ | 5 / 10 / 150 | up to 1,000 by total paid | 2 / 10 / 40 / 100 |
| Persistent login | Whole browser incl. memory | Contexts: cookies, localStorage, IndexedDB, service workers; **not** sessionStorage or HTTP cache | Profiles: cookies, storage, extensions, settings; **open tabs do not carry over**; 300 MB | Profiles: cookies, local, session storage and cache | Profiles and managed auth | Not stated on pricing page (`not-published`) | Persisted sessions |
| Fork a running logged-in browser | **Yes** | No fork/clone in docs | No fork/clone in docs | Not documented (`not-published`) | No (vendor reading) | Not documented (`not-published`) | No |
| Checkpoint / rewind a browser | **Yes** (Pro, Scale) | No | No | No | No | No | No |
| Captcha | Click by default; solve (reCAPTCHA v2/v3, hCaptcha, Turnstile, GeeTest, image) | Auto captcha solving on paid plans | $3/1k (Launch, after $10 verified), $1/1k (Scale) | Automatic captcha solving | Stealth mode incl. captcha solver, all plans | Included | Included, all plans |
| Live view / human takeover | Live view; takeover with audit row | Live view, click/type/scroll, human-in-the-loop | Live view (send a link) | Not stated on pricing page (`not-published`) | Live view, all plans | Not stated on pricing page (`not-published`) | Stream pages (Hybrid Automations) |
| Managed login vault / TOTP | **Yes**, write-only vault with TOTP | Not stated on pricing page | No | No | Managed auth, vaults | No | No |
| Regions | Ashburn only (Amsterdam "coming back later") | US East, US West, EU Central, UK | Not on pricing page (`not-published`) | Not on pricing page (`not-published`) | Regional on Start-Up | US default; EU enterprise | SF, London, Amsterdam |
| Own sandboxes on the same key | **Yes** (VMs) | Sandboxes (Hyperbrowser-style credit); Browserbase has Runtime/Functions | No | Yes (sandboxes) | Apps | Browser Harness open source | No |
| Proxy | `egress`/`country` fields, no proxy product | 1 GB–5 GB included, $10–12/GB after | $10/GB, $6/GB | $10/GB | Managed proxy in stealth | $5/GB home IP, $0.20/GB direct | Included |

Sources and quotes (each found live on 2026-11-26):

- Browserbase — https://www.browserbase.com/pricing: "then $0.12/browser hr"; "then $0.10/browser hr"; "25concurrent browsers"; "100browser hours"; "100concurrent browsers"; "500browser hours"; "3concurrent browsers"; "1browser hour"; "15minutes /session". Tag `pricing-page`. Keep-alive: "Session keep alive is only available on paid plans."; "Keep alive sessions must be explicitly released to stop; otherwise, they'll continue until the session's timeout." — https://docs.browserbase.com/features/keep-alive, `docs-confirmed`. Contexts: "Contexts don't include the browser's HTTP cache (images, CSS, JS, fonts)."; "Contexts don't save sessionStorage." — https://docs.browserbase.com/features/contexts, `docs-confirmed`. Live view: "watch, click, type, and scroll in real-time"; "Human in the loop - instantly take control or provide input" — https://docs.browserbase.com/features/session-live-view, `docs-confirmed`. Fork: the word "fork" or "clone" appears nowhere in https://docs.browserbase.com/features/sessions, https://docs.browserbase.com/reference/api/overview or https://docs.browserbase.com/llms.txt (checked by search) — `inferred`.
- Steel — https://steel.dev/pricing: "Browser hours | $0.10 | $0.08 | Custom"; "Concurrent browser sessions | 10 | 100 | 1,000+"; "Max session time | 15 minutes | 1 hour | Up to 24 hours"; "CAPTCHA solves | $3/1k | $1/1k | Custom". Tag `pricing-page`. Profiles: "Open tabs do not carry over." — https://docs.steel.dev/overview/profiles-api/overview, `docs-confirmed` (Styne's own comparison says "Profiles up to 300 MB, deleted after 30 days unused").
- Hyperbrowser — https://www.hyperbrowser.ai/pricing: "Browser usage | 100 credits / hour | $0.10 / hour"; "25 concurrent sessions"; "100 concurrent sessions"; "Automatic CAPTCHA solving"; "1 concurrent session". Tag `pricing-page`. Profiles: "which includes cookies, local storage, session storage, and cache, across multiple sessions" — https://hyperbrowser.ai/docs/sessions/profiles, `docs-confirmed`.
- Kernel — https://www.kernel.sh/pricing: "$0.0000166667 per GB-second (no charges for idle time)"; "5 concurrent browsers"; "10 concurrent browsers"; "150 concurrent browsers"; "live view"; "stealth mode (captcha solver & proxy)"; "managed auth (3 connections)". Tag `pricing-page`. https://kernel.sh/docs/info/pricing: "Headless browser | 0.0000166667"; "Headful browser | 0.0001333336"; "You're charged only while the browser is active. Once the browser enters standby, usage charges stop and the `timeout_seconds` countdown begins." — `pricing-page` / `docs-confirmed`. **Our conversion, not Kernel's:** $0.0000166667 a GB-second × 3,600 s = $0.0600 an hour for a 1 GB headless browser; × 8 = $0.48 for a headful browser if it is 8 GB (the headful rate is 8× the headless rate, which suggests 8 GB, but Kernel does not state the headful memory on that page — `inferred`).
- Browser Use Cloud — https://browser-use.com/pricing: "$0.06per browser hour"; "Up to 1,000 browsers at once"; "CAPTCHA solving**Included**"; "billed by the minute, 1-minute minimum"; "Credits never expire." Tag `pricing-page`.
- Browserless — https://www.browserless.io/pricing: "Browser time  8 hours" (Free); "165 hours" (Prototyping, $25 annual); "Longest session | 2 min | 15 min | 30 min | 60 min"; "Automatic captcha solving | Included | Included | Included | Included"; "Persisted sessions and replays | 1 day | 7 days | 30 days | 90 days". Tag `pricing-page`.

### Styne's own "Styne vs Browserbase" page, checked against Browserbase

The lead noted Styne's footer links its comparison pages (https://styne.ai/compare links E2B, Daytona, Modal, Vercel and Browserbase). I read the Browserbase one and checked every Browserbase price and limit it states against Browserbase's own page today:

| Styne's claim about Browserbase | Browserbase's own page | Verdict |
|---|---|---|
| "$0.12 on Developer, $0.10 on Startup" | "then $0.12/browser hr"; "then $0.10/browser hr" | Matches |
| "Developer $20 a month: 100 browser hours, 25 concurrent browsers" | "100browser hours"; "25concurrent browsers" on the $20 Developer card | Matches |
| "Free plan: 1 browser hour, 3 concurrent browsers, 15-minute sessions" | "1browser hour"; "3concurrent browsers"; "15minutes /session" | Matches |
| "Startup $99 a month: 500 browser hours, 100 concurrent browsers" | "500browser hours"; "100concurrent browsers" | Matches |
| "Contexts keep the Chromium user data directory … Not sessionStorage or the HTTP cache" | "Contexts don't save sessionStorage."; "Contexts don't include the browser's HTTP cache" | Matches |
| "No fork or clone of a running browser" | No fork/clone in the sessions page, API overview or llms.txt | Consistent (inferred from absence) |
| "US East, US West, EU Central and UK" | Not found on the pricing page we read | `not-published` here; not confirmed |

Styne's page does not state a date for the Browserbase facts in its table beyond "read on 2026-11-18". Its timing comparison (Styne 29 ms create vs Browserbase 845 ms round trip) compares different things, and Styne says so; it is not usable as a head-to-head.

### Price arithmetic (computed here, not copied from any vendor)

- **Extra hour after a plan's hours.** Styne $0.04 against Browserbase $0.12 / $0.10, Steel $0.10 / $0.08, Hyperbrowser $0.10, Kernel $0.06 (1 GB headless) to $0.48 (8 GB headful), Browser Use $0.06, Browserless ~$0.18–0.24. Styne is the cheapest per-hour list price after its included hours, and the only one that bills $0 while the browser sleeps with its memory intact.
- **Entry plan included hours per dollar.** Styne $20 → 350 h = $0.057/h all-in if fully used; Browserbase $20 → 100 h = $0.20/h; Steel Launch is credit-only; Hyperbrowser $30 → 30 h = $1.00/h; Kernel $0 → $5 credit ≈ 83 h at 1 GB headless; Browserless $25 annual → 165 h = $0.15/h.
- **A day of intermittent work, one browser.** Suppose a browser is awake 6 hours in a 24-hour day, twice a day. Styne: 6 h × $0.04 = $0.24, asleep the rest. Browserbase Developer: 6 h × $0.12 = $0.72, and only if each session is released; keep-alive bills until released. Kernel: 6 h × $0.06 = $0.36. Browser Use: 6 h × $0.06 = $0.36. **The saving is real but small** (cents a day at one browser); it matters at hundreds of browsers or with long idle gaps, not at small scale.
- **Where Styne's sleep actually saves money.** It is the difference between paying for wall-clock time and paying for awake time. A browser that must stay signed in overnight with no work to do costs $0 on Styne and on Kernel (standby), but on Browserbase, Steel and Browser Use it costs until released or timed out. This is the one feature whose value grows with idle time.

## 3. Search API comparison

Per 1,000 searches, each returning up to about 10 results.

| | **Styne** | **Brave Search API** | **Exa** | **Tavily** | **Firecrawl** | **Hyperbrowser Web API** | **Browserbase Search** |
|---|---|---|---|---|---|---|---|
| Price per 1,000 searches (10 results) | $1 fast / $2 auto / $5 deep | $5 | $4 (lite) / $7 (auto, fast) | $8 basic / $16 advanced (at $0.008 a credit) | Search 2 credits per 10 results; $19 Hobby = 5,000 credits → ~$0.0076 a search at that tier | $5 a query | $1 Parallel Fast / $5 Parallel Advanced / $7 Exa Fast / $12 Exa Deep |
| Fetch per 1,000 pages | $1 | n/a | $1 per content type | $1.60 basic (1 credit per 5 pages) | 1 credit a page | $1 ($6 with JSON) | $1 |
| Free | Prepaid credit only, no free tier on search | $5 credit a month | $10 credit a month | 1,000 credits a month | 1,000 credits a month | 5,000 credits with a free account | 1,000 calls on paid plans |
| Drop-in for other code | Tavily and Google Custom Search | — | — | — | — | — | — |
| Result shape | Ranked results with best-passage text, date, licence | Web, LLM context, news, images | Page contents with results | Results plus optional answer | Search plus scrape | Search plus fetch | Wraps Parallel and Exa |

Sources: https://docs.styne.ai/search ("| `fast` | $0.001 | $1 |"; "| `auto` | $0.002 | $2 |"; "| `deep` | $0.005 | $5 |"; "Eleven to twenty results are two blocks.") `docs-confirmed`; https://api-dashboard.search.brave.com/documentation/pricing ("per 1,000 requests"; "Includes free $5 in credits every month.") `pricing-page`; https://exa.ai/docs/admin/pricing ("| `/search` (`lite`) | $4 / 1k requests |"; "| `/search` (`auto`, `fast`) | $7 / 1k requests |"; "| `/contents` | $1 / 1k pages, per content type |") `pricing-page`; https://docs.tavily.com/documentation/api-credits ("Pay-as-you-go**: $0.008 per credit"; "Each request costs **1 API credit**."; "Each request costs **2 API credits**.") `docs-confirmed`; https://www.tavily.com/pricing ("$0.008"; "1,000 API credits / month") `pricing-page`; https://www.firecrawl.dev/pricing ("Search costs 2 credits per 10 results"; "Hobby: $19/month billed monthly") `pricing-page`; https://www.hyperbrowser.ai/pricing ("Search | 5 credits / query | $0.005 / query") `pricing-page`; https://www.browserbase.com/pricing ("$1/1k for Parallel Fast", "$5/1k for Parallel Advanced", "$7/1k for Exa Fast", "$12/1k for Exa Deep") `pricing-page`.

**Our arithmetic:** Tavily basic at the pay-as-you-go rate is 1 credit × $0.008 = $0.008 a search, i.e. $8 per 1,000; advanced is $16. Firecrawl at $19 for 5,000 credits (1 credit = 1 page; a 10-result search is 2 credits) is 5,000 ÷ 2 = 2,500 searches → $19 ÷ 2.5 = $7.60 per 1,000.

**Verdict on search.** Styne's search is mid-priced and honest at its docs rate: cheaper than Tavily and Brave at every mode, close to Exa lite at $1 vs $4 and Exa auto at $2 vs $7, dearer than Firecrawl's per-credit math at its cheapest tier, and tied with Browserbase's own Parallel-Fast passthrough at $1. Its one distinctive offering is the Tavily and Google Custom Search drop-in endpoints on the same key as the sandboxes and browser. That is convenience for someone already on Styne, not a reason to choose Styne. Its "own index" claim is not corroborated (see the `web:brave` field) and its headline price is mislabelled. Nothing here warrants a BotPerch table.

## 4. What providers already on BotPerch offer a browser or desktop

From `site/src/data/capabilities.json` (`browser` and `desktop` rows, both under `built-in-agents`) and `features/built-in-agents.json`.

`browser` — "A web browser (Chrome or Chromium) ready for an agent to drive."

29 rows: 10 `yes`, 6 `partly`, 13 `no` (counted from the file).

- **`yes` (10):** agent37, blaxel, boat, boxd, daytona, instabox, lizard, runtime, sandboxer, tensorlake.
- **`partly` (6):** e2b (desktop image or Kernel cloud browsers), modal (documented computer-use example), morph (example only), sprites (Chrome only through Cursor), digitalocean (Omarchy image or Harness sessions), freestyle (guide installs Chromium).
- **`no` (13):** cloudflare-sandbox, vercel-sandbox, hetzner, hostinger, hostup, ovhcloud, lightsail, vultr, contabo, linode, scaleway, tierhive, hosthatch.

`desktop` — "A graphical desktop an agent can see and click": `yes` for agent37, blaxel, boat, boxd, daytona, e2b, instabox, lizard, morph, runtime, sandboxer, tensorlake, digitalocean; `partly` for modal, sprites, freestyle, hostup, ovhcloud, lightsail, vultr, linode, scaleway; `no` for cloudflare-sandbox, vercel-sandbox, hetzner, hostinger, contabo, tierhive, hosthatch.

The feature file's idea is explicit: "A preinstalled agent is not a hosted agent. Preinstalled means the binary is on the disk … Hosted means the provider runs the loop and bills you for it." And the `browser` capability's plain line is about a browser *in the machine*: "A web browser (Chrome or Chromium) ready for an agent to drive."

### How Styne's browser compares with these

- **It is a hosted browser product, not a preinstalled browser in your sandbox.** Every existing `yes` row is a browser that ships in the provider's image or template and that you drive inside a machine you also use for code. Styne's is a separate, first-class product: one Chromium per microVM, started by API, metered as browser hours, with live view, takeover, vault and fork of the *browser*. It is closest to Runtime's `sbx.browser.start()` returning a CDP address (Runtime is already `yes`), but Styne goes further: the browser survives sleep in memory, forks logged in, and keeps per-end-user profiles.
- **It has browser features no existing row claims.** No provider on BotPerch documents a write-only login vault with TOTP, per-end-user browser profiles with hosted sign-in links, step verification, browser checkpoint/rewind, or Browserbase/Steel-compatible endpoints. InstaBox and boxd document human takeover of a desktop; Browserbase and Kernel are not on BotPerch.
- **It lacks what most existing `yes` rows have.** A desktop (no `desktop` capability), an in-machine browser you use next to your code (you get that from Styne's separate VM product instead, on the same key), proxy bandwidth, and more than one region.
- **Against the dedicated services, its distinctive combination is:** $0 sleep with the whole browser in memory + fork a logged-in browser + checkpoint/rewind (paid plans) + live view and takeover + vault with TOTP + step verification, at the lowest extra-hour price, on the same key as its own sandboxes. Each piece exists elsewhere (Kernel standby, Browserbase live view, Kernel managed auth, Steel profiles, Hyperbrowser sandboxes); the combination and the price do not.
- **For whom it differentiates:** teams running agents on *logged-in* sites (customer portals, SaaS dashboards, internal tools) where a session must stay signed in between bursts of work, where a person sometimes has to step in for a captcha or a 2FA, and where parallel logged-in copies save re-login. Not for: anyone needing EU or Asia regions today, anyone needing proxy bandwidth bundled, anyone who needs fork/checkpoint on a $20 plan, anyone wanting a free tier without a card, or a user who only needs a browser inside a coding sandbox (they already have ten `yes` rows).

## 5. Recommendation for BotPerch

The existing fields can carry all of this; no schema change is needed. Two advisor-review corrections apply: the search-price discrepancy is reported as written rather than resolved into a corrected unit, and the `browser` capability gets `partly`, not `yes`.

### a) `browser` capability row — verdict `partly`

The advisor review is right that a plain `yes` would misrepresent the row. The capability sits in the `built-in-agents` group ("A web browser (Chrome or Chromium) ready for an agent to drive"), and every existing `yes` row is a browser in the provider's own machine, used next to your code. Styne's browser is a separately metered cloud product. A `partly` row with a plain-language `shortfall` keeps the fact visible without changing what the column means. `site/src/data/capabilities.json`, the `browser` capability, new row:

```json
{
  "provider": "styne",
  "verdict": "partly",
  "figure": "Cloud browser; $0.04/h",
  "shortfall": "A hosted cloud browser on its own meter, not a browser in your sandbox: you get one by calling the browser API, and it bills $0.04 an hour while awake (350 hours included on the $20 plan). Styne's own sandbox templates ship no browser: its docs say the browser template \"does not start a sandbox\".",
  "note": "Chromium in its own microVM, driven over CDP, with a dashboard live view, human takeover, per-end-user profiles and a login vault. Idle browsers sleep at $0 and wake on the same tab, and a logged-in browser can be forked. Checkpoint and rewind need the $99 plan.",
  "source": {
    "url": "https://docs.styne.ai/browsers",
    "quote": "A browser session is a Chromium instance in its own microVM that you drive over the Chrome DevTools Protocol (CDP) with Playwright, Puppeteer, Stagehand or Browser Use.",
    "confidence": "docs-confirmed"
  }
}
```

Do not use a bare `yes`. If the owner would rather change the row's meaning to "any browser for an agent, hosted or in the machine", that is a deliberate re-interpretation of the `built-in-agents` group and should be argued and applied to the whole column, not smuggled in through one new provider. A second source worth adding to the profile's `sources` is https://styne.ai/pricing ("Each extra browser hour", "$0.04", `pricing-page`). The `shortfall`'s second sentence is sourced: https://docs.styne.ai/guide-templates, "The list may also show a browser template. It is for browsers, and it does not start a sandbox." (`docs-confirmed`, read live 2026-11-26).

### b) Provider profile `strengths` — two lines

Plain language, in the profile's voice:

1. "A cloud browser product of its own: each browser is a Chromium in its own microVM, driven over CDP, with a live view, human takeover for a login or captcha, and a write-only login vault that types passwords and two-step codes. Idle browsers sleep at no charge and wake on the same tab, and a signed-in browser can be forked into parallel copies. Checkpoint and rewind come with the $99 plan."
2. "The browser and the sandbox share one account and one key, and the browser API answers the Browserbase and Steel session endpoints, so existing client code runs after a base URL change."

A `fits.badly` line is worth having too: "Needs a region outside the US East today, or wants proxy bandwidth in the plan: browsers run only in Ashburn, Amsterdam is 'coming back later', and proxy traffic is not a product."

### c) `caveats` — the search price contradiction

Use the existing `Caveat` shape with `status: "contradiction"` and both statements:

```json
{
  "issue": "The search price headline disagrees with the billing docs",
  "detail": "The pricing page's headline reads \"$0.001 / 1,000 searches in fast mode\", captioned \"Per started block of 10 results\", but the same page's FAQ, the billing docs and the search docs give $1, $2 and $5 per 1,000 searches (fast / auto / deep) for blocks of 10 results. The two disagree as written; which is billed cannot be settled from Styne's pages. Use the docs figure.",
  "status": "contradiction",
  "evidence": [
    { "url": "https://styne.ai/pricing", "quote": "$1 per 1,000 searches in fast mode, $2 in auto and $5 in deep", "confidence": "pricing-page" },
    { "url": "https://docs.styne.ai/billing", "quote": "per 1,000 blocks", "confidence": "docs-confirmed" }
  ]
}
```

Both quotes were machine-checked against the live pages: the pricing-page FAQ sentence ("Search is billed per started block of 10 results: $1 per 1,000 searches in fast mode, $2 in auto and $5 in deep.") and the billing table's "$5 per 1,000 blocks". The headline card itself reads "$0.001" above "/ 1,000 searches in fast mode" — quote that pair if a second pricing-page evidence entry is wanted.

### d) `browser-agent` use case (`site/src/data/usecases.json`)

- Add a **route**, tier `preinstalled` (the browser is ready, nothing to install), `noTerminal: "partly"` (an API key and one `POST /browsers` call; the dashboard can start one):

  > "Start a cloud Chromium with `POST /browsers` and pass its `cdp_url` to Playwright or Puppeteer. The browser sleeps at $0 with its tabs, cookies and memory kept, wakes on the same page, and can be forked into logged-in copies; a dashboard live view lets a person take over for a login or captcha."

  Link: "Styne: browsers" → https://docs.styne.ai/browsers. `price`: "$20 a month with 350 browser hours, then $0.04 an hour; sleeping browsers cost $0".

- Add a **pick**:

  > `for`: "Browsers that stay logged in"
  > `why`: "A Chromium per microVM that sleeps at $0 with its tabs and memory kept, forks into parallel logged-in copies, and has a live view with human takeover and a login vault for passwords and two-step codes."
  > `watch`: "Only US East (Ashburn); Amsterdam is coming back later. Checkpoint and rewind need the $99 plan, and there is no proxy bandwidth in the plan."
  > `link`: https://styne.ai/browsers

- In `outside`, leave the browser-services sentence as it is. The search API belongs here only if the owner wants it, as one added sentence: "Styne also sells a search, fetch and crawl API on the same key, billed from prepaid credit at $1, $2 or $5 per 1,000 searches." I would not add it: it is a different product from the browser, and the use case is about driving a browser.

### e) What not to do

- **Do not add a search-API comparison table or a new feature group.** BotPerch compares machines and what runs in them; the search API does not change any sandbox verdict, and Styne's index claim is not corroborated.
- **Do not cite Styne's timing numbers** (29 ms, 135 ms, 30 of 30) as evidence. If the owner wants them, label them "Styne's own measurement, 2026-11-18".
- **Do not repeat "on our own index."** The docs' `web:brave` source field is the only hint of what the index is, and Styne does not explain it.
- **Do not repeat Styne's "$0.001 / 1,000 searches" headline.** Use $1/$2/$5 per 1,000 searches.

### f) Freshness for integration

- All Styne pages read live 2026-11-26; no page shows a "last updated" date. Styne's own dates in the text: pricing configuration 2026-11-20; browser comparison 2026-11-15; measurements 2026-11-18; the Amsterdam region is described as "coming back later" — check it before publishing, because a second region would change the EU sentence.
- Styne's browser docs page is long and was updated between my two reads of the day (it lists an extension upload dated 2026-11-23); re-read before integration.

## 6. Gaps and what would settle them

| Gap | Status | Smallest check |
|---|---|---|
| Whether Styne's search index is its own | `not-published`; docs show `web:brave` | Ask Styne, or read one `results[].source` from a live call (needs an account and credit: not authorised) |
| Hyperbrowser and Browser Use fork/clone, live view, session length | `not-published` on their pricing pages | Their docs pages for sessions; not needed for a BotPerch verdict |
| Steel regions | `not-published` on the pricing page | Steel's regions docs |
| Browserbase regions (Styne lists four) | Not on the pricing page we read | Browserbase regions docs |
| Kernel headful browser memory | `inferred` (8 GB from the 8× rate) | Kernel browser docs |
| Styne's real behaviour (sleep, fork, takeover) | Documented only | Would need an account and billable browsers: not authorised |
