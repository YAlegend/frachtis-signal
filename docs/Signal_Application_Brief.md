# Signal — proof-of-work for the Frachtis *Investor / AI Agent Builder* role

**Repo:** https://github.com/YAlegend/frachtis-signal · runs offline (`--demo`), in a browser (Run
button), and in the cloud (GitHub Actions). Built with Claude Code.

Rather than apply with a folder of demo agents, I built **the AI associate stack a crypto pre-seed
fund should have** — one coherent system that sources dealflow, scores it against the fund's encoded
thesis, writes source-cited memos, and then **tests whether its own sourcing signal is reliable**. It
is built on Frachtis's own worldview: every output is *verifiable* (source-linked), the pipeline runs
within *defined rules* (a thesis/policy layer), and it *collapses a manual workflow*.

---

## How it maps to what the role tests

**Venture judgment.** The fund's thesis is *encoded*, not assumed — `config/thesis.yaml` weights the
themes Frachtis backs (agent control planes 1.0 → consumer/social 0.5), each with an `open_question`
the memo agent is forced to answer. I built a *system* (sourcing → synthesis → reliability test), not
four toys, because the judgment signal is understanding that the durable winners are the control
planes that make agents reliable — so the tool itself is one.

**Crypto-native research.** Signals are tuned to where crypto/AI founders actually emerge *before*
they trend: GitHub star-*velocity* (computed from snapshots, exposed as a custom MCP server I own),
IACR/arXiv research, on-chain traction (DefiLlama + Blockscout), Farcaster reputation (OpenRank), and
hackathon/grant footprints. Memos are **source-linked**, and unciteable claims are dropped — the
"read primary sources, not summaries" discipline, enforced in code.

**AI agent building.** It's shipped and runnable in three ways, with an **eval harness gating CI** —
a scorer change merges only if precision/recall hold (seven offline checks gate every push). Model
routing is provider-agnostic (cheap/local triage → frontier synthesis), with a rate-limit throttle so
batch runs don't silently degrade. I can name the production failure modes because I hit and fixed
them (below).

---

## The centerpiece: I built the *test* of the tool, and let it tell the truth

The hardest, most honest thing here is the **Phase 0 backtest**. It reconstructs *point-in-time*
signal (GitHub via GH Archive, sites via the Wayback Machine — no look-ahead) and asks whether Signal
would have surfaced real deals *early enough to matter*. The verdict was **🟡 YELLOW**:

- **67% recall on public-channel deals** — it genuinely catches the open-source / public slice…
- **0% on warm-intro / inbound deals** — …and misses relationship-sourced deals, which is how most
  pre-seed actually closes.
- **45-day median lead time** on the deals it did catch.

I deliberately **did not chase a clean GREEN, and did not fabricate point-in-time data** to get one.
The finding is the product: public-signal sourcing is a real edge on a *specific* slice and a blind
spot elsewhere — so the strategy is to lean sourcing where it provably works and put the
higher-leverage bet on **synthesis** (the Memo agent). That is the "opinions about what works in
production vs. what only looks good in a demo" the role asks for, expressed as evidence rather than a
claim.

---

## What's actually built (and what's honestly not)

- **Six signals**, free + premium tiers, blended into one 0–100 score with sub-scores shown so a
  reviewer sees *why*. Two **convergence twins** — `network_radar` (credible Farcaster accounts newly
  converging) and Nansen Smart-Money (credible wallets newly converging) — the same "who's early"
  thesis on the social and on-chain layers.
- **Memo agent** — company → structured, source-cited investment memo in the fund's voice.
- **Web UI** (a real Run button, Live/Demo toggle, scorer picker, signal toggles) and a **cloud Run
  button** via GitHub Actions.
- **Graceful by design:** premium sources (Harmonic, Messari, Nansen, Neynar, Evertrace) are
  **scaffolded and fixture-tested but not live-verified** — without a paid key each contributes 0 and
  nothing breaks. I'd rather call that "scaffolded" than imply it's proven. The free tier is fully
  working and tested.

**Production lessons, fixed in the code:** hallucinated citations → a verifier drops them;
all-LLM was slow/expensive → parsing is plain code, the model is reserved for judgment; free-tier
rate limits → throttle + retry; and a scoring bug where the model's *informational* notes ("early,
no recent commits") were penalised like a scam → I split hard anti-signals from soft risks so a
high-fit early team surfaces with visible risks instead of being zeroed.

---

## What this says about how I'd operate at Frachtis

I encode a thesis and make it tunable; I build the test, not just the thing; I let the data redirect
the strategy instead of defending the build; and I keep the model on a leash — reserved for judgment,
every claim verifiable. That is the associate I'd be: high-agency about shipping, low-ego about what
the evidence says.

**Try it in 60 seconds:** `pip install pyyaml && PYTHONPATH=src python -m signalfund --demo` →
`out/digest.md`. Or open the repo's Actions tab → *Sourcing run* → *Run workflow*.
