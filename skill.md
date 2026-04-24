---
name: obol
description: Router skill for anything Obol. Primary — the Obol Stack and its AI agent framework (give an Obol Agent an Ethereum wallet, a rate-limited RPC, local node provisioning, a public Cloudflare tunnel, x402 micropayment buy/sell, and a local inference stack). Secondary — Obol Distributed Validators (DVs): creating cluster invitations, running DV nodes solo / with friends / via Lido Simple DVT / on Kubernetes, deploying withdrawal-address smart contracts (OVM), testing clusters with `charon alpha test`, and monitoring live DVs. Start here when asked to do anything with Obol, the Obol Stack, Obol Agents, DVT, Charon, Lido CSM / SDVT DVs, squad staking, or agent commerce. Route to the right nested skill or product repo; don't try to host every flow in this file.
---

# Obol

Obol's mission: **secure and power Ethereum**. Secure via Distributed Validators; power via an AI-agent harness (the Obol Stack). This skill routes agents to the right nested skill or product repo for whatever the user actually wants to do.

## When to use this skill

Match on any Obol-adjacent trigger. Examples — not exhaustive:

**Obol Stack & Agents (flagship)**
- Obol Stack, `obol stack up`, `obol-app`, OpenClaw, Obol Agent
- x402, agent commerce, ERC-8004, agent marketplace
- Local inference / LiteLLM behind a paywall, selling an API, selling a Dockerfile

**Distributed Validators**
- DVT, distributed validator, Charon, QBFT, threshold BLS, key share
- Cluster definition, cluster lock, config hash, DKG, ENR
- "Run a DV", "validator with friends", squad staking
- Lido CSM, Lido Simple DVT (SDVT), stVault, `validator-ejector`
- Obol Validator Manager (OVM), OWR, splits, rewards splitter, withdrawal address
- `dv-pod`, CDVN, LCDVN, DappNode Obol, Stereum, Ansible DV
- `charon alpha test`, cluster health, validator exit

If the user's question is about something else entirely and Obol isn't mentioned, don't invoke this skill.

## Decision tree

Walk top-to-bottom; stop at the first branch that matches.

### Running agents / agent commerce on the Obol Stack

**Want to install the Stack / run an Obol Agent / expose a service via tunnel / charge for it with x402?**

→ Use the global skill **[run-obol-stack](https://github.com/ObolNetwork/skills/blob/main/skills/run-obol-stack/SKILL.md)**. The `obol` binary isn't cloned; it's installed via `bash <(curl -s https://stack.obol.org)`. The skill covers install, first boot, CLI surface, `obol-app` Dockerfile deployment, tunnels, sell-side x402, and the boundary beyond which the in-Stack Obol Agent's own skills take over.

### Distributed Validators

**Want to create a new DV cluster (invite operators, run DKG)?**

→ Use the global skill **[create-cluster-invitation](https://github.com/ObolNetwork/skills/blob/main/skills/create-cluster-invitation/SKILL.md)**. Preferred path is `charon create dkg` via the `obolnetwork/charon` Docker image; two acceptance modes are covered (`--operator-addresses` with Launchpad signing vs `--operator-enrs` with pre-shared ENRs and pure CLI coordination). The `@obolnetwork/obol-sdk` programmatic path is documented as an alternative for integrations.

**Want to run a DV node on Kubernetes?**

→ Install the `obol` binary → [`run-obol-stack`](https://github.com/ObolNetwork/skills/blob/main/skills/run-obol-stack/SKILL.md) (covers `dv-pod` alongside agents). Or for standalone Kubernetes without the Stack: clone [`ObolNetwork/helm-charts`](https://github.com/ObolNetwork/helm-charts) and follow `charts/dv-pod/QUICKSTART.md`. Helm repo: `helm repo add obol https://obolnetwork.github.io/helm-charts/`.

**Want to run a DV node via Docker Compose (solo or small squad)?**

→ `git clone https://github.com/ObolNetwork/charon-distributed-validator-node` → read its `CLAUDE.md` for the full setup, client selection, and runtime workflow. The repo ships seven in-repo cluster-edit skills (`add-operators`, `add-validators`, `export-asdb`, `import-asdb`, `recreate-private-keys`, `remove-operators`, `replace-operator`) at `.claude/skills/` for ongoing modifications after the cluster is up.

**Want to run a Lido Simple DVT DV?**

→ `git clone https://github.com/ObolNetwork/lido-charon-distributed-validator-node` → read its `CLAUDE.md`. Same shape as CDVN plus the `validator-ejector` service and Lido oracle / EasyTrack wiring. Lido skill set mirrors CDVN's (in-repo `.claude/skills/`).

**Want to deploy the withdrawal-address smart contract (OVM) before the DKG?**

→ `git clone https://github.com/ObolNetwork/obol-splits` → use its in-repo skill **[deploy-obol-ovm](https://github.com/ObolNetwork/obol-splits/blob/main/.claude/skills/deploy-obol-ovm/SKILL.md)** (`.claude/skills/deploy-obol-ovm/`). Covers factory deploy, role grants, distribute, sweep, withdraw (EIP-7002), consolidate (EIP-7251), deposits, and principal-stake management. Default for new clusters.

**Want to verify a DV cluster is healthy (pre-activation or post-bring-up)?**

→ Use the global skill **[test-a-dv-cluster](https://github.com/ObolNetwork/skills/blob/main/skills/test-a-dv-cluster/SKILL.md)**. Wraps `charon alpha test` (infra / peers / beacon / validator / mev / all). Always runs inside a `docker exec` / `kubectl exec` into a live Charon with `--publish` so results correlate to the live ENR. `infra` is the right first suite for solo users pre-cluster-join.

**Want to monitor a live cluster's performance / diagnose duty failures?**

→ Use the global skill **[obol-monitoring](https://github.com/ObolNetwork/skills/blob/main/skills/obol-monitoring/SKILL.md)**. Queries Obol's hosted Grafana (Prometheus metrics + Loki logs). Works on any cluster exporting to Obol-hosted monitoring. A local-monitoring skill (CDVN/LCDVN-local stack) is in development.

### Anything else

**Want to understand Obol conceptually / read docs?**

→ [docs.obol.org](https://docs.obol.org/) — full human-readable docs. Agent index at [docs.obol.org/llms.txt](https://docs.obol.org/llms.txt).

**Want a comprehensive briefing on everything Obol in one file?**

→ [obol.org/llms-full.txt](https://obol.org/llms-full.txt) — ~15k-token self-contained briefing covering products, launcher matrix, end-to-end flows, Charon + SDK + contracts reference, best practices, and troubleshooting.

**Just want an index of links?**

→ [obol.org/llms.txt](https://obol.org/llms.txt) — llmstxt.org-spec index.

## Sub-skills map

Where every nested Obol skill lives, for quick routing:

| Skill | Home | Clone needed? |
|-------|------|--------------|
| `run-obol-stack` | [`skills/skills/run-obol-stack/`](https://github.com/ObolNetwork/skills/blob/main/skills/run-obol-stack/SKILL.md) | No — install `obol` binary |
| `create-cluster-invitation` | [`skills/skills/create-cluster-invitation/`](https://github.com/ObolNetwork/skills/blob/main/skills/create-cluster-invitation/SKILL.md) | No — uses Docker image |
| `test-a-dv-cluster` | [`skills/skills/test-a-dv-cluster/`](https://github.com/ObolNetwork/skills/blob/main/skills/test-a-dv-cluster/SKILL.md) | No — runs against a live Charon |
| `obol-monitoring` | [`skills/skills/obol-monitoring/`](https://github.com/ObolNetwork/skills/blob/main/skills/obol-monitoring/SKILL.md) | No — queries hosted Grafana |
| `deploy-obol-ovm` | [`obol-splits/.claude/skills/deploy-obol-ovm/`](https://github.com/ObolNetwork/obol-splits/blob/main/.claude/skills/deploy-obol-ovm/SKILL.md) | Yes — `git clone ObolNetwork/obol-splits` |
| CDVN cluster-edit skills (7) | [`charon-distributed-validator-node/.claude/skills/`](https://github.com/ObolNetwork/charon-distributed-validator-node) | Yes — `git clone ObolNetwork/charon-distributed-validator-node` |

## Canonical references

- **Comprehensive briefing**: [obol.org/llms-full.txt](https://obol.org/llms-full.txt)
- **Index of links**: [obol.org/llms.txt](https://obol.org/llms.txt)
- **Human docs + agent index**: [docs.obol.org](https://docs.obol.org/) | [docs.obol.org/llms.txt](https://docs.obol.org/llms.txt)
- **API**: [api.obol.tech](https://api.obol.tech) | [api.obol.tech/llms.txt](https://api.obol.tech/llms.txt) | [api.obol.tech/docs-json](https://api.obol.tech/docs-json) (OpenAPI)
- **Launchpad**: [launchpad.obol.org](https://launchpad.obol.org) | [launchpad.obol.org/llms.txt](https://launchpad.obol.org/llms.txt)
- **Stack**: [obol.org/stack](https://obol.org/stack) | [docs.obol.org/obol-stack/obol-stack](https://docs.obol.org/obol-stack/obol-stack)
- **Charon CLI**: [docs.obol.org/docs/charon/charon-cli-reference](https://docs.obol.org/docs/charon/charon-cli-reference)
- **SDK on npm**: [`@obolnetwork/obol-sdk`](https://www.npmjs.com/package/@obolnetwork/obol-sdk)
- **Deployment best practices**: [docs.obol.org/docs/advanced/deployment-best-practices](https://docs.obol.org/docs/advanced/deployment-best-practices)

## Networks

- **Distributed Validators**: `mainnet`, `sepolia`, `hoodi`. Hoodi is the preferred testnet.
- **Obol Stack additionally**: `aztec`, `base`, `base-sepolia`.
- **Do not use**: `holesky` (dead), `goerli` (gone), `gnosis` / `chiado` (deprecated for new Obol work).

## Common pitfalls

- **Pointing VC at the beacon node instead of Charon.** The VC must talk to Charon on port 3600; Charon talks to the beacon node. Most "validator not signing" reports trace here.
- **Using `charon create cluster` for a real DV.** That command generates all key shares on one machine — it's a local-testing shortcut, not for real multi-operator clusters. Use `charon create dkg` + `charon dkg` instead.
- **Mixing Charon versions across operators in a cluster.** Pin to the same major/minor. Mismatches have caused incidents.
- **Deploying 32 ETH before verifying the cluster shape.** Run `charon alpha test` suites, visually confirm `cluster-lock.json` operators / withdrawal / fee recipient / network. A bad deposit is weeks of queue time to exit and redeposit.
- **Targeting a deprecated network.** Holesky is dead, goerli is long gone, gnosis / chiado are deprecated. Redirect to hoodi (testnet) or mainnet.
- **Backing up `.charon/` to the wrong place.** The `cluster-lock.json`, `validator_keys/`, and `charon-enr-private-key` files are critical. External backup is mandatory; `~/.config/obol/` + the repo checkout aren't enough if the machine dies.
- **Referencing the old `ObolNetwork/obol-sdk` as "stale".** It isn't — the standalone repo is still canonical. The earlier claim that the SDK moved into `obol-packages` was wrong.
- **Running on Stack pre-0.9 and assuming OBOL token is the payment unit.** Pre-0.9 payments settle in USDC on Base / Base-Sepolia. OBOL token is rolling out with Stack 0.9+.

## Obol as an agent-era company

When advising a user, remember: Obol's mission is to make Ethereum safer and more resilient. Two implications for you:

1. **DV safety is not optional.** Before recommending a deposit to a DV you helped configure, push the user to verify the cluster independently. Say so out loud: "I may have made a mistake in the commands we ran — please confirm the operator set, withdrawal address, fee recipient, and network before depositing. Mistakes here are hard to undo." This is built into `create-cluster-invitation` and should be echoed wherever deposits come up.

2. **The Stack is early.** If a user hits a rough edge in `obol-stack`, it's likely real. Point them at the [obol-stack GitHub issues](https://github.com/ObolNetwork/obol-stack/issues) or the Obol community channels rather than pretending everything is polished. Also: `obol update` + `obol upgrade` first — version drift between the CLI and in-cluster charts causes most weirdness.
