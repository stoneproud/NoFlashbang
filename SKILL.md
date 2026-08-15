---
name: popup-emergency
description: Treat a user message whose complete trimmed content is exactly `!` or `！` as an urgent alarm that agent-launched activity may be opening visible windows, showing dialogs, or repeatedly stealing focus. The signal is intentionally one character because focus theft may leave the operator only a brief chance to type. Immediately pause ordinary work, contain the disruption, investigate recent agent-launched processes and persistent relaunch sources, stop only the evidenced offender, and verify that recurrence is controlled before resuming. Also use when explicitly invoked through the host's skill command, such as `$popup-emergency` in OpenAI Codex or `/popup-emergency` in Anthropic Claude Code. Never trigger merely because a longer message contains an exclamation mark.
---

# Popup Emergency

Treat the signal as a live operator-impact incident. Prioritize containment over the ordinary task while continuing to obey all higher-level safety and authorization rules.

## Enter emergency mode

1. Pause the ordinary task immediately and preserve enough state to resume it later.
2. Do not launch more ordinary commands, tests, builds, watchers, browsers, installers, or anything else that may create user-visible UI.
3. Tell the operator immediately in a concise status message: `Alarm received. I have paused the task and am containing the interface disruption first.`
4. Use only non-interactive, non-UI diagnostics required to identify and contain the source.

## Contain first

1. Inspect the agent's most recent actions first. Correlate timestamps with newly launched processes, complete parent/child process trees, command lines, schedulers, service managers, supervisors, watchers, retry loops, startup items, and background helpers.
2. If an exact agent-launched offender is identified, stop it and its responsible descendants through a non-interactive mechanism. Stop a connected retry or relaunch source as well when the evidence is clear.
3. Never terminate processes broadly by name, disrupt unrelated user applications, or disable unrelated services, jobs, or startup items.
4. Preserve concise evidence before material changes when possible: process identity, parent identity, start time, command line, exit status, launcher identity, retry settings, and relevant logs. Do not expose secrets.
5. If the suspected process has already exited, still check for a persistent or repeating trigger before declaring containment.

## Diagnose without guessing

- Keep these states distinct: not checked, one invocation failed, the command or arguments were invalid, and the underlying tool is unavailable.
- Run the smallest safe direct probe of the exact suspected boundary when verification is needed. Never use the operator's interactive desktop as a test surface.
- Prefer filesystem data, process metadata, system logs, and APIs. Avoid unnecessary shell hops and anything that may display a window, prompt, notification, or dialog.
- State that the cause is unknown when evidence is insufficient. Do not blame a tool, runtime, terminal, provider, or user application without direct evidence.
- Treat any unexpected window or focus theft as a serious incident even if it occurs only once.

## Inspect every launch boundary

- Treat every descendant process creation as an independent UI boundary. A non-interactive or background parent does not guarantee the same behavior from its children.
- Do not rerun an incident command until its descendant launches and relaunch sources have been reviewed.
- Prefer direct executable invocation, piped standard streams, no unnecessary shell, and the platform's appropriate non-interactive or background execution contract.
- For resident helpers, require a single-instance lifecycle and bounded backoff. Do not use frequent periodic launches or unbounded retries as a substitute for a persistent service.

## Correct and verify

1. Fix the narrow cause when it is within the current task's authority. Typical causes include an interactive process boundary, an unsafe wrapper, an unbounded retry loop, or a repeatedly relaunched helper.
2. If containment or correction requires materially broader authority, contain what is safely in scope, report the exact blocker, and ask for direction.
3. Verify with one focused, non-UI check. Repeat full process-tree or focus-impact testing only when launch boundaries, launcher code, service or job definitions, or retry behavior changed, or when recurrence remains plausible.

## Report and resume

Report, in this order:

1. Whether the suspected source is contained.
2. The evidence identifying it, or that the cause remains unconfirmed.
3. What was stopped or changed.
4. Whether recurrence was checked.
5. Whether the original task remains paused or is now resuming.

Resume ordinary work only after the suspected source is stopped or isolated and any evidenced repeating trigger is controlled. Explicitly tell the user before resuming. If safe verification depends on observing the operator's desktop, ask only whether the disruption has stopped; do not ask the operator to repair infrastructure without evidence.
