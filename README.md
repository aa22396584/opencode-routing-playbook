# OpenCode Routing Playbook

> **Development home:** https://github.com/ImL1s/opencode-routing-playbook  
> Please open issues and pull requests there.  
> **Mirrors:** [Codeberg](https://codeberg.org/ImL1s/opencode-routing-playbook) · [GitLab](https://gitlab.com/aa22396584/opencode-routing-playbook)


Reusable, sanitized OpenCode + oh-my-openagent routing profiles for different subscription and API-key combinations.

This repo is intentionally **profile-first**: do not treat one config as universal. Pick a profile based on what the user actually has: GPT/OpenAI OAuth, OpenAI API key, OpenCode Go, Gemini/Google, Cursor local router, or a provider-diverse mix.

Need a rough comparison against a pure Claude Opus 4.7 setup? See [`docs/opus47-relative-estimates.md`](docs/opus47-relative-estimates.md) for profile-by-profile quality, speed, resilience, and CP/value ranges.

No real API keys or tokens belong in this repo.

## Quick choose

| If you have... | Start with... | Why |
| --- | --- | --- |
| GPT/OpenAI OAuth + OpenCode Go | `daily-gpt55-go-provider-diverse` | Quality-first daily setup: GPT primary, Go for specialist/CP/fallback lanes. |
| GPT/OpenAI OAuth only | `gpt55-oauth-openai-only` | Same-provider OpenAI profile with GPT-5.5 where locally executable. |
| OpenAI API key + OpenCode Go | `provider-diverse-safe-gpt54-go` | Portable API-safe profile; does not assume GPT-5.5 API-key availability. |
| OpenCode Go + small OpenAI API budget | `go-first-openai-api-review` | Spend Go first, keep OpenAI for escalation/final review. |
| OpenAI API only, sensitive work | `openai-only-sensitive` | Same-provider fallbacks only; no Go/Google/Cursor routing. |
| OpenAI API only, cost-sensitive | `openai-api-budget-mini` | GPT-5.4-mini first, GPT-5.4 only for hard lanes. |
| OpenCode Go only | `opencode-go-only-balanced` | Uses GLM/Kimi/MiMo/MiniMax/Qwen by role. |
| OpenCode Go only, cheapest sweeps | `opencode-go-only-low-cost` | MiniMax/Qwen-heavy bulk lane. |
| OpenCode Go only, huge context | `opencode-go-only-large-context` | MiMo large-context-first lane. |
| OpenCode Go only, multimodal | `opencode-go-only-multimodal` | MiMo-Omni-first image/audio/video/PDF lane. |
| Gemini/Google only | `google-gemini-only` | Gemini-only same-provider profile. |
| Gemini/Google + OpenCode Go | `google-gemini-go-provider-diverse` | Gemini primary with Go context/cheap/multimodal lanes. |
| Cursor local router only | `cursor-local-router-only` | OpenCode front-end over Cursor local/ACP model router. |
| Cursor local router + OpenCode Go | `cursor-go-provider-diverse` | Cursor primary with Go side lanes. |

## Profile capabilities at a glance

This is the short human read before opening a profile directory. For rough Opus 4.7-relative percentages, see [`docs/opus47-relative-estimates.md`](docs/opus47-relative-estimates.md).

| Profile | Capability posture | Strongest lanes | Avoid / caveat |
| --- | --- | --- | --- |
| `daily-gpt55-go-provider-diverse` | Best default when GPT subscription + OpenCode Go are both available. | Core coding, planning, review, Go cheap/context/multimodal side lanes, provider fallback. | Cross-provider; not ideal for sensitive repos unless accepted. |
| `gpt55-oauth-openai-only` | High-quality same-provider GPT subscription profile. | Daily coding/review where GPT-5.5 OAuth is locally executable. | No Go fallback; do not assume GPT-5.5 API-key availability. |
| `provider-diverse-safe-gpt54-go` | Portable OpenAI API + Go mixed profile. | GPT-5.4 for reliable work; Go for context, cheap sweeps, multimodal, outage fallback. | API spend can grow if GPT is used for every lane. |
| `go-first-openai-api-review` | Cost-first mixed profile with OpenAI as escalation. | Bulk Go work, long-context exploration, OpenAI final review/escape hatch. | Not ideal when every task needs premium final quality. |
| `openai-only-sensitive` | Same-provider OpenAI profile for approved-provider boundaries. | Sensitive/proprietary repos where OpenAI is allowed and cross-provider is not. | Lower CP/fallback than provider-diverse profiles. |
| `openai-api-budget-mini` | OpenAI mini-first budget profile. | Low-risk edits, summaries, small helper lanes, cost control. | Do not use mini-first output as final authority for complex code. |
| `opencode-go-only-balanced` | General-purpose OpenCode Go-only setup. | Broad non-sensitive work using GLM/Kimi/MiMo/MiniMax/Qwen by role. | Quality below premium GPT/Opus for hard final review. |
| `opencode-go-only-low-cost` | Cheapest high-volume Go profile. | Sweeps, classification, summarization, exploration, batch docs. | Not for hard debugging, architecture, or final code review. |
| `opencode-go-only-large-context` | Large-context-first Go profile. | Big repos, large logs, long documents, context-heavy audits. | Wasteful if context size is not the bottleneck. |
| `opencode-go-only-multimodal` | Multimodal-first Go profile. | Image/audio/video/PDF and media-heavy understanding. | Use balanced Go or GPT profiles for ordinary text/code work. |
| `google-gemini-only` | Gemini-only same-provider profile. | Gemini-first reasoning/search where Google auth is available. | Template: verify local model IDs and quotas before relying on it. |
| `google-gemini-go-provider-diverse` | Gemini reasoning + Go specialist lanes. | Gemini for core reasoning; Go for context, cheap sweeps, multimodal side work. | Cross-provider prompts need privacy acceptance. |
| `cursor-local-router-only` | OpenCode UI over Cursor local router/ACP. | Users who already rely on Cursor subscription/router and want OpenCode-style routing. | Depends on local Cursor endpoint and current router model list. |
| `cursor-go-provider-diverse` | Cursor primary with Go side lanes. | Cursor core coding plus Go cheap/context/multimodal lanes. | Cross-provider; needs both local Cursor router and Go access. |

## Profiles shipped

`verified` means smoke/list checked in the originating local setup. `template` means installable JSON that still needs `opencode models <provider>` and a smoke test on the target machine.

| Profile | Status | Providers | Main model | Best fit |
| --- | --- | --- | --- | --- |
| `daily-gpt55-go-provider-diverse` | verified | openai, opencode-go | `openai/gpt-5.5` | daily local workflow when GPT-5.5 is actually executable and OpenCode Go is available |
| `gpt55-oauth-openai-only` | template | openai | `openai/gpt-5.5` | ChatGPT/Codex-style OAuth users who can run GPT-5.5 but do not want cross-provider routing |
| `provider-diverse-safe-gpt54-go` | verified | openai, opencode-go | `openai/gpt-5.4` | users with OpenAI API plus OpenCode Go, without assuming GPT-5.5 API availability |
| `go-first-openai-api-review` | template | opencode-go, openai | `opencode-go/glm-5.1` | users with small OpenAI API budget and cheap Go capacity |
| `openai-only-sensitive` | verified | openai | `openai/gpt-5.4` | sensitive work where OpenCode Go or other provider fallback is not acceptable |
| `openai-api-budget-mini` | template | openai | `openai/gpt-5.4-mini` | OpenAI API users optimizing cost for low-risk repos/tasks |
| `opencode-go-only-balanced` | template | opencode-go | `opencode-go/glm-5.1` | users who only have OpenCode Go and want a general-purpose setup |
| `opencode-go-only-low-cost` | template | opencode-go | `opencode-go/minimax-m2.7` | bulk summarization, classification, exploration, low-risk docs |
| `opencode-go-only-large-context` | template | opencode-go | `opencode-go/mimo-v2.5-pro` | large repo sweeps, big logs, long documents, context-heavy audits |
| `opencode-go-only-multimodal` | template | opencode-go | `opencode-go/mimo-v2-omni` | vision-heavy, media-heavy, and document-understanding workflows |
| `google-gemini-only` | template | google | `google/gemini-3.1-pro-preview` | users with Gemini/Antigravity access but no OpenAI/OpenCode Go |
| `google-gemini-go-provider-diverse` | template | google, opencode-go | `google/gemini-3.1-pro-preview` | Gemini-first users who also have Go for CP/context lanes |
| `cursor-local-router-only` | template | cursor-acp, cursor | `cursor-acp/auto` | users who want OpenCode UI/routing over an existing Cursor subscription |
| `cursor-go-provider-diverse` | template | cursor-acp, cursor, opencode-go | `cursor-acp/auto` | Cursor users who also have OpenCode Go and want cheaper/faster side lanes |

## Project layout

```text
configs/<profile>/
  opencode.json             # installable sanitized OpenCode config
  oh-my-openagent.json      # role/category model routing and fallback chains
  README.md                 # profile-specific fit/install/smoke notes
docs/
  profile-selection.md      # decision tree by subscription/API situation
  subscription-matrix.md    # full combination matrix
  opus47-relative-estimates.md # rough profile percentages versus pure Claude Opus 4.7
  profile-architecture.md   # naming, profile tiers, and contribution rules
  model-rationale.md        # why each model family appears
  security.md               # secret and provider-boundary guidance
profiles.json               # machine-readable catalog
scripts/install_profile.sh  # validates, then installs with backups and chmod 600
scripts/validate_configs.py # JSON/index/fallback/provider-boundary/secret checks
.github/workflows/validate.yml # CI guard for public profile changes
```

## Install

Preview/validate first:

```bash
python3 scripts/validate_configs.py configs/opencode-go-only-balanced
scripts/install_profile.sh --dry-run configs/opencode-go-only-balanced
```

Install with backup:

```bash
scripts/install_profile.sh configs/opencode-go-only-balanced
```

Then verify locally:

```bash
opencode debug config
opencode run -m opencode-go/minimax-m2.7 --title routing-smoke --dangerously-skip-permissions 'Reply with exactly: OK'
```

## Important assumptions

- `fallback_models` is an **oh-my-openagent** routing feature; plain OpenCode may not honor these chains by itself.
- `openai` and `opencode-go` are treated as OpenCode/auth/plugin-managed providers. That is why the public templates do not declare custom `provider.openai` or `provider.opencode-go` blocks.
- Custom provider blocks are kept minimal and profile-specific. A same-provider profile should not register unrelated provider endpoints.
- Optional MCP servers are intentionally not bundled into every profile. Add local MCPs after selecting the model-routing profile.
- Cross-provider profiles can send prompt/code to multiple providers. Use `openai-only-sensitive`, `google-gemini-only`, or `cursor-local-router-only` when a single approved provider boundary matters.
- GPT-5.5 is kept in OAuth/subscription-oriented profiles because it was locally executable in the originating setup. Do not assume GPT-5.5 API-key availability in portable API profiles.
- Model lists, availability, pricing, and context windows drift. Re-run `opencode models <provider>` and smoke tests before presenting a profile as current for another machine.

---

## Support

If this project saved you some time, you can [buy me a coffee](https://buymeacoffee.com/iml1s).
