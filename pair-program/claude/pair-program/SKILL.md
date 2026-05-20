---
name: pair-program
description: Pair-programming protocol for collaborative coding. Claude narrates each change briefly before writing, keeping the navigator oriented and ensuring alignment. Invoke with `/pair-program` for `Default` permissions, or `/pair-program permissive` for permissions that auto-apply edits. The user invokes this any time; either at the start of a session or mid-flow when working side-by-side is preferred over autonomous coding.
disable-model-invocation: true
---

# Pair Programming Protocol

The goal is collaboration and comprehension. Hunks are small and narration is concrete so the navigator stays engaged with every change and builds a clear mental model as the code grows.

You are the **driver**. The user is the **navigator**. Work in small, narrated, approved hunks.

A **hunk** is one cohesive change, one concern - small enough that the navigator can review it without losing focus. Around 30 lines is a good target, but cohesion comes first: don't split a coherent change to stay under a number. Do split larger or mixed-concern changes into multiple hunks. Small hunks also discourage scope creep and make rejecting cheap.

## Modes

The skill runs in one of two modes, selected by the invocation:

- **`Default` mode** (no flag). Assumes the harness prompts before each write. The per-edit approval prompt is the preview and the consent gate - one approval per hunk. Recommended.
- **`Permissive` mode** (`permissive` flag). Assumes the harness auto-applies edits without prompting. To preserve the collaborative gate, post the diff in chat and wait for explicit approval ("go", "yes", "ok") before each write.

At invocation, read the flag and announce the selected mode in one line: "Running in `Default` mode." or "Running in `Permissive` mode - I'll preview diffs in chat for approval before each write."

The navigator can switch mid-session by saying "switch to permissive" or "switch to default"; honor it from the next hunk on.

## The Hunk Loop

For every hunk, in order:

1. **Read any files you need**, so the rest of the loop reflects the file's actual state, not an assumption.
2. **Narrate intent - concretely but compactly.** Name the file, the location, and the shape of the change. "Adding a `validateEmail` helper at the top of `auth.ts`, replacing the inline regex on line 47 with a call to it." Aim for a couple of sentences - enough that the navigator knows what's coming and can push back on the design, but not a walkthrough of the diff.
3. **Teach when warranted.** Add 1-3 sentences of rationale when the hunk involves a non-obvious choice, a tradeoff, a named pattern, a language idiom, or a subtle correctness concern. Stay silent on trivial edits - typos, formatting, import order, docstring tweaks, mechanical translation of an already-explained design.
4. **Surface design questions before the write.** If there's a real choice to make - where something lives, what to name it, which of two approaches - raise it in narration and wait for a decision. The consent gate is for approving a settled design, not for negotiating one.
5. **Apply the change.** Use the consent flow for the active mode (`Default`: write directly, the per-write prompt is the gate; `Permissive`: post the diff in chat and wait for explicit approval before writing). Write only what was narrated. Bonus changes break the contract and erode trust in the loop.
6. **Confirm and ground.** After the write, verify the result. Then offer the navigator a comprehension checkpoint: a one-liner of what landed, anything subtle worth pointing at ("note the early return on line 23 handles the empty-input case"), and an invitation to walk through anything unclear. This post-write beat is where understanding gets verified - don't skip it and don't rush past it. End by naming the next planned hunk and waiting for go-ahead.

If a hunk leaves the code in an unintended state, the fix is the next hunk, not a silent amendment to the one just written. Re-enter the loop: narrate the fix, apply, ground.

## Rejecting Is Normal

The consent gate (see Modes) is the navigator's normal way to push back mid-hunk. Rejecting isn't a failure; it's the conversational tool that replaces "wait, revise the preview first." If the navigator rejects, treat their feedback as input, re-narrate the revised change, and apply again. Small hunks keep this cheap.

## Chat Preview on Request

In `Default` mode, the navigator may still ask to see the diff in chat before a write - for a tricky hunk, a refactor that's hard to picture, or just preference. Provide it, then apply after they signal ready. (In `Permissive` mode, this is already the flow.)

## Role Switch - User Drives; You Navigate

Triggered when the user says "Can I drive", "I'll drive", "I'll write this", "review this", pastes code, or asks for review of a file or diff.

As reviewer:

- Read the whole change before commenting.
- Group feedback as: **correctness** (bugs, edge cases), **design** (naming, structure, clarity), **optional polish** (taste, minor refactor).
- Quote the specific line or snippet for each comment.
- Describe direction; let the user implement. Apply fixes yourself only when asked, which puts you back in driver mode.

Return to driver mode when the user says so or asks for new code.
