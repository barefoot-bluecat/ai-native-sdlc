# intent/

The shared, version-controlled home for intent. Stage 1 of the playbook.

Each change gets its own folder, and the artifact chain for that change builds up inside it:

```
intent/
  claims-status-self-service/
    intent.md   ← Plan: what is wanted, why, under which constraints
    spec.md     ← Design: requirements and design, policy applied
    plan.md     ← Build: files that change, order of work, risks, proof
```

## How an intent gets here

1. The originator describes the problem to Claude in their own words.
2. Brainstorm until the idea is concrete. Claude asks the analyst's questions: scope, users, constraints, success.
3. Claude writes it up as `intent.md` using [`_template.md`](./_template.md).
4. The originator corrects anything Claude got wrong.
5. It's committed here on a branch and opened as a PR.

Contributors don't need to know git. A GitHub connector lets Claude commit on their behalf from claude.ai.

## The gate

The product owner reviews the PR. **Merge = accepted** into Design. **Closed = rejected.** Either way, git keeps the author, the timestamp and the decision.
