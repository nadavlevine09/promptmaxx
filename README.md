# promptmaxx

A Claude skill that challenges a rough prompt, rewrites it into a sharper one, and then runs the improved version.

## Why

Most bad results come from the prompt, not the model. A prompt can be vague, leave out the output format, have no success criteria, or ask the wrong question entirely. promptmaxx makes Claude review the prompt *before* doing the work, so you get a better answer and see why it's better.

## How it works

Start a message with `/promptmaxx` followed by your rough prompt. Claude will:

1. **Challenge the framing.** Is this the right question? Are there hidden assumptions, a false either/or choice, or a request that treats a symptom instead of the real problem? If the framing is fine, it says so and moves on instead of inventing objections.
2. **Find the gaps.** Missing deliverable, no output format, unclear scope or audience, no examples, too much or too little context, a multi-step task that isn't broken into steps.
3. **Rewrite the prompt.** Clear task up top, concrete format instructions, examples or structure only where they help, filler cut, and your original intent kept.
4. **Show its work.** A short note on any framing issues, then the improved prompt in a code block with a list of what changed and why.
5. **Run it.** Claude carries out the improved prompt. If you only wanted the rewritten text (for example, to paste into another tool), it stops after step 4. If the challenge changes what should be built, it asks you before going ahead.

## Example

```
/promptmaxx write a python script that emails me every time my website goes down
```

Claude might point out that a hosted uptime monitor already does this without you running a script, ask what "down" means (no response? error codes? slow pages?), then rewrite the prompt with check intervals, alert conditions, retry logic, and a clear deliverable, and build it.

## Also triggers on

- "improve / optimize / sharpen / tighten this prompt"
- "make this a better prompt"
- pasting a prompt and asking Claude to clean it up before running it

It does **not** trigger on everyday requests that just happen to be vague, only when you ask for the prompt itself to be improved.

## Install

**Claude Code:**

```bash
git clone https://github.com/nadavlevine09/promptmaxx ~/.claude/skills/promptmaxx
```

**Claude.ai:** zip the folder and upload it under Settings → Capabilities → Skills.
