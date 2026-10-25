# ai-native-sdlc

A reference build of Anthropic's [AI-native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook), implemented play by play, as written.

The stack is the one an Azure-first enterprise would run: Claude Code with models served through **Microsoft Foundry** (Entra ID auth), **GitHub** for source control and CI. Nothing here is tied to a real organisation. It's a worked example.

## The loop

Every stage ends by committing an artifact. The next stage begins by reading it. The chain of commits is the audit trail: who asked for what, what the agent produced, and who approved it.

```
intent.md ──► spec.md ──► plan.md ──► diff + tests ──► PR + review findings ──► deploy
    ▲                                                                              │
    └──────────────── breached control band writes a new intent.md ◄───────────────┘
```

| Stage    | Artifact                     | Gate (human)                          |
| -------- | ---------------------------- | ------------------------------------- |
| Plan     | `intent/<change>/intent.md`  | Product owner accepts (merge)         |
| Design   | `intent/<change>/spec.md`    | Product owner signs off               |
| Build    | `intent/<change>/plan.md`    | Engineer accepts the plan             |
| Test     | tests + verification output  | Code owner reviews the PR             |
| Deploy   | PR with review findings      | Code owner approval, release authority |
| Maintain | new `intent.md` from a breach | Service owner triages                 |

## Build status

The playbook's dependency graph sets the order. Plays get ticked off as they land.

| # | Stage    | Play                                   | Status |
| - | -------- | -------------------------------------- | ------ |
| 0 | —        | Foundations (repo, Foundry, branch protection) | 🟡 in progress |
| 1 | Plan     | Capture as `intent.md`                 | ⬜ |
| 2 | Build    | The `CLAUDE.md`                        | ⬜ |
| 3 | Build    | Skills as institutional knowledge      | ⬜ |
| 4 | Design   | Requirements and design → `spec.md`    | ⬜ |
| 5 | Build    | Plan mode as the default starting point | ⬜ |
| 6 | Build    | Hooks as build-time guardrails         | ⬜ |
| 7 | Build    | Parallel sessions and subagents        | ⬜ |
| 8 | Test     | Give Claude a feedback loop            | ⬜ |
| 9 | Test     | Continuous evals in CI                 | ⬜ |
| 10 | Deploy  | AI in the PR review loop (`REVIEW.md`) | ⬜ |
| 11 | Deploy  | Hooks as approval gates                | ⬜ |
| 12 | Deploy  | CI/CD integration and deployment       | ⬜ |
| 13 | Maintain | Closing the loop on metrics           | ⬜ |
| 14 | Maintain | Recurring codebase scans              | ⬜ |
| 15 | Maintain | Claude on call                        | ⬜ |

Skills come before Design because the Design play lists brand, security, compliance and UX skills as a prerequisite.

## Running Claude Code against Foundry

```bash
cp .env.foundry.example .env   # fill in your resource and deployment names
az login                       # Entra ID: no API key stored anywhere
set -a; source .env; set +a
claude                         # then /status — provider should read Microsoft Foundry
```

`.env` is gitignored, and Claude is denied read access to it in `.claude/settings.json`.

## Layout

```
intent/            Stage 1 home. One folder per change: intent.md, spec.md, plan.md
.claude/           Shared Claude Code config: settings, skills, agents, hooks
CODEOWNERS         Who approves what
```

`CLAUDE.md` and `REVIEW.md` arrive with their own plays, not before.
