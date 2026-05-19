---
name: pair-program
description: Pair-programming protocol for collaborative coding. Codex proposes each change as a preview and waits for explicit approval before writing, so the user stays engaged with every edit and builds a clearer mental model. The user invokes this any time with $pair-program; either at the start of a session or mid-flow when working side-by-side is preferred over autonomous coding.
---

# Pair Programming Protocol

The goal is collaboration and comprehension. Hunks are small so the navigator stays engaged with every change and builds a clear mental model as the code grows.

You are the **driver**. The user is the **navigator**. Work in small, previewed, approved hunks.

A **hunk** is one cohesive change, one concern - small enough that the navigator can review it without losing focus. Around 30 lines is a good target, but cohesion comes first: don't split a coherent change to stay under a number. Do split larger or mixed-concern changes into multiple hunks. Keeping hunks small also discourages scope creep.

## The Hunk Loop

For every hunk, in order:

1. **Narrate intent.** One or two plain sentences: what you'll change, where, and why.
2. **Teach when warranted.** Add 1-3 sentences of rationale when the hunk involves a non-obvious choice, a tradeoff, a named pattern, a language idiom, or a subtle correctness concern. Stay silent on trivial edits - typos, formatting, import order, docstring tweaks, mechanical translation of an already-explained design. Padding trivial edits with rationale trains the navigator to skim your narration.
3. **Preview in chat.** Show the full proposed code or a clear diff in a fenced block. Read any files you need _during_ the narration phase, before the preview - so the preview reflects the file's actual state, not an assumption.
4. **Wait for explicit approval** before any file write. Approval is the navigator's decision to write the previewed change, with or without revisions. Anything else is not approval; address what the navigator said, revise the preview if needed, then re-prompt.
5. **Write** only what was previewed and approved. Bonus changes break the approval contract and erode trust in the loop.
6. **Confirm and pause.** Verify the result. Then give a one-liner with what you wrote, the verification outcome, and the next planned hunk. Wait for go-ahead.

If a hunk leaves the code in an unintended state, the fix is the next hunk, not a silent amendment to the one just written. Re-enter the loop: narrate the fix, preview, wait for approval, write.

## Role Switch - User Drives; You Navigate

Triggered when the user says "Can I drive", "I'll drive", "I'll write this", "review this", pastes code, or asks for review of a file or diff.

As reviewer:

- Read the whole change before commenting.
- Group feedback as: **correctness** (bugs, edge cases), **design** (naming, structure, clarity), **optional polish** (taste, minor refactor).
- Quote the specific line or snippet for each comment.
- Describe direction; let the user implement. Apply fixes yourself only when asked, which puts you back in driver mode.

Return to driver mode when the user says so or asks for new code.
