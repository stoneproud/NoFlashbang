# NoFlashbang

> A cross-platform popup-emergency Agent Skill for OpenAI Codex and Anthropic Claude Code.

NoFlashbang contains user-interface disruption caused by agent-launched activity. A message containing only `!` becomes an emergency signal: pause the current task, identify the responsible process or relaunch source, contain it safely, and verify that the disruption will not recur before resuming.

The skill does not assume that one coding agent is safer or more capable than another. Any agent with permission to launch local processes can accidentally cross an interactive UI boundary. The same containment procedure applies to Codex, Claude Code, and other hosts that implement the Agent Skills format.

## Why the name NoFlashbang

An unexpected agent-launched window is more than a harmless rectangle. It can appear at full brightness, cover the operator's current work, seize keyboard focus, and interrupt input before a sentence can be finished. From the operator's side, that feels like a small software flashbang: sudden light, disorientation, interruption, and a momentary loss of control.

`NoFlashbang` names the execution contract this skill defends. Background automation should stay in the background unless the operator explicitly requests visible interaction. No surprise windows. No focus theft. No retry loop throwing the same flashbang again.

When that contract fails, `!` is the shortest emergency signal the operator may still be able to send.

## Why the trigger is one character

A process that repeatedly opens windows or steals focus can make normal typing physically difficult. The operator may have only a brief moment in which the agent input regains focus, so a long command is not a reliable emergency control.

`!` is intentionally short enough to type and submit during that moment. The full-width form `！` is accepted for users whose input method produces it. Exact matching prevents ordinary sentences containing exclamation marks from triggering emergency mode.

## Compatibility

The portable behavior lives entirely in `SKILL.md`, using the shared Agent Skills structure: YAML frontmatter with `name` and `description`, followed by Markdown instructions.

| Host | Personal installation | Project installation | Explicit invocation |
| --- | --- | --- | --- |
| OpenAI Codex | `$HOME/.agents/skills/popup-emergency` | `<repo>/.agents/skills/popup-emergency` | `$popup-emergency` |
| Anthropic Claude Code | `$HOME/.claude/skills/popup-emergency` | `<repo>/.claude/skills/popup-emergency` | `/popup-emergency` |

Both hosts can also activate the skill implicitly when the complete message is `!` or `！`. The repository is named `NoFlashbang`, but the installed skill directory should remain `popup-emergency` so its command name is predictable across hosts.

`agents/openai.yaml` supplies optional OpenAI UI metadata. It does not contain the emergency workflow and is not required by Claude Code.

## Trigger rules

Emergency activation on either host occurs when the complete message, after trimming surrounding whitespace, is exactly:

- `!`
- `！`

Explicit activation uses the host's normal syntax:

- OpenAI Codex: `$popup-emergency`
- Anthropic Claude Code: `/popup-emergency`

Messages such as `Build finished!` do not trigger emergency mode.

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

Copy or clone this repository into a directory named `popup-emergency` at one of the host locations in the compatibility table. Use the personal location to make the skill available across projects, or the project location to share it through version control.

To use one checkout with both hosts, create host-supported symbolic links from both skill locations to the same `popup-emergency` directory. Copying the directory twice also works, but later updates must be synchronized manually.

The installed directory should contain:

```text
popup-emergency/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

If a newly created top-level skill directory is not detected, restart the host. Both products otherwise support discovering skill-file changes without rebuilding the skill.

## Usage

If agent-launched activity starts opening windows or stealing focus, send:

```text
!
```

The agent should pause the original task, acknowledge the alarm, contain the evidenced source, report what it found and changed, verify recurrence, and only then resume.

Under normal conditions, invoke the skill explicitly with `$popup-emergency` in Codex or `/popup-emergency` in Claude Code.

## Limitations

This skill is an emergency response procedure, not an operating-system kill switch. Its effectiveness depends on the agent having access to safe process inspection and containment tools. If the disruptive UI prevents any message from reaching the agent, the trigger cannot activate until the operator can submit it.

## Format references

- [OpenAI: Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Anthropic: Extend Claude with skills](https://code.claude.com/docs/en/skills)
- [Agent Skills open standard](https://agentskills.io)
