# NoFlashbang

> Popup emergency containment for Codex.

NoFlashbang is a cross-platform Codex skill for containing user-interface disruption caused by agent-launched activity. It turns a message containing only `!` into an emergency signal: pause the current task, identify the responsible process or relaunch source, contain it safely, and verify that the disruption will not recur before resuming.

## Why the trigger is one character

A process that repeatedly opens windows or steals focus can make normal typing physically difficult. The operator may have only a brief moment in which the Codex input regains focus, so a long command is not a reliable emergency control.

`!` is intentionally short enough to type and submit during that moment. The full-width form `！` is accepted for users whose input method produces it. Exact matching prevents ordinary sentences containing exclamation marks from triggering emergency mode.

## Trigger rules

The skill activates when the complete message, after trimming surrounding whitespace, is exactly:

- `!`
- `！`
- `$popup-emergency` when explicitly invoking the skill

Messages such as `Build finished!` do not trigger it.

## What it does

1. Pauses ordinary task work immediately.
2. Avoids launching anything else that may create visible UI.
3. Inspects the agent's recent actions, process descendants, background helpers, schedulers, services, supervisors, watchers, and retry loops.
4. Stops only the evidenced offender and any confirmed relaunch source.
5. Preserves useful evidence and avoids blaming tools without verification.
6. Checks that recurrence is controlled before resuming the original task.

## Safety model

The skill follows four principles:

- **Contain first:** stop ongoing operator impact before pursuing a complete diagnosis.
- **Use evidence:** distinguish an untested hypothesis, a failed invocation, invalid arguments, and an unavailable tool.
- **Change narrowly:** never terminate processes broadly by name or disable unrelated jobs and services.
- **Verify before resuming:** inspect repeating triggers and the complete descendant launch chain.

Higher-level system policies, user authorization, and platform security controls always take precedence.

## Cross-platform design

The skill deliberately avoids operating-system-specific commands. It describes the required outcome in terms of processes, descendant launches, services, schedulers, startup items, retry sources, logs, and non-interactive execution boundaries. The agent should use the safest platform-native inspection and containment mechanisms available in its environment.

A background parent process is not assumed to make its children safe. Every descendant launch boundary must independently satisfy the platform's non-interactive execution contract.

## Installation

Place this directory at:

```text
$CODEX_HOME/skills/popup-emergency
```

If `CODEX_HOME` is not configured, place it under the `.codex/skills` directory in your user home. Restart or reload Codex if your environment does not discover newly added skills automatically.

The installed directory should contain:

```text
popup-emergency/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

## Usage

If agent-launched activity starts opening windows or stealing focus, send:

```text
!
```

The agent should pause the original task, acknowledge the alarm, contain the evidenced source, report what it found and changed, verify recurrence, and only then resume.

You can also invoke the skill explicitly in a normal message with `$popup-emergency`.

## Limitations

This skill is an emergency response procedure, not an operating-system kill switch. Its effectiveness depends on the agent having access to safe process inspection and containment tools. If the disruptive UI prevents any message from reaching Codex, the trigger cannot activate until the operator can submit it.
