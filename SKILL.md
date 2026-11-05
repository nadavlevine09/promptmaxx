---
name: promptmaxx
description: Challenges and rewrites a rough or underspecified prompt — questioning whether the framing, assumptions, and scope are even right, not just polishing wording — then executes the improved version. Triggers whenever the user's message starts with "/promptmaxx" (treat everything after it as the raw prompt to improve). Also use when the user explicitly asks to "improve," "optimize," "sharpen," "tighten," "challenge," or "rewrite" a prompt before running it, asks "make this a better prompt," or pastes a prompt/instruction and asks Claude to clean it up before acting on it. Do NOT trigger on ordinary requests that just happen to be vague — only on the "/promptmaxx" command or when the user signals they specifically want the prompt itself improved, not just the task done.
---

# Promptmaxx

Takes a prompt the user is about to send (to this conversation, to an API call, to a sub-agent, to another tool), rewrites it to be sharper, then acts on the improved version. Always show the improved prompt — never rewrite silently.

## Workflow

1. **Identify the raw prompt.** If the user's message starts with `/promptmaxx`, everything after it is the raw prompt (if nothing follows, ask what they want improved). Otherwise it's pasted directly, referenced ("the prompt above"), or is the user's own message when they say something like "make this a better prompt."

2. **Challenge the prompt before polishing it.** Don't just accept the task as framed — interrogate it:
   - Is this the right question, or is there a better one underneath it? (e.g. "write me a script to do X" when the real goal would be better served by not doing X at all)
   - Are there unstated assumptions baked in that might be wrong? Surface them.
   - Is the requester asking for a solution to a symptom rather than the underlying problem?
   - Is there a hidden false dichotomy, an unnecessary constraint, or scope that's too narrow/broad for the actual goal?
   - Would a domain expert push back on part of this before executing it?
   - If nothing's actually wrong with the framing, say so briefly — don't manufacture objections.
   This step produces callouts, not silent edits: flag the issue and state how you resolved it (reframed, flagged, or kept as-is) rather than quietly changing the user's intent.

3. **Diagnose mechanical gaps.** Check for:
   - Missing or vague task definition (what's the actual deliverable?)
   - No success criteria / output format specified
   - Ambiguous scope (word count, depth, audience, tone)
   - No examples where they'd disambiguate intent
   - Buried the actual ask under irrelevant context, or vice versa (too little context)
   - Multi-step tasks that should be broken into explicit steps
   - Missing constraints (what NOT to do) if that's a recurring failure mode

4. **Rewrite applying best practices:**
   - State the task in one clear sentence up top
   - Add concrete output-format instructions (length, structure, format)
   - Add 1-2 examples if they'd meaningfully disambiguate
   - Use XML tags or headers to separate distinct pieces (context vs. instructions vs. constraints) if the prompt is long or multi-part
   - Ask for step-by-step reasoning if the task is analytical/multi-step
   - Cut filler — every sentence should change the output
   - Preserve the user's actual intent; don't scope-creep the task

5. **Show the challenge, then the before/after.** Lead with any framing pushback from step 2 (one or two sentences — don't write an essay). Then present the improved prompt in a code block with a brief list of the mechanical changes made and why.

6. **Then act on the improved prompt** as the actual instructions for the rest of the turn, unless the user only wanted the rewritten text itself (e.g., they're going to paste it into a different tool/API — in that case, stop after step 5). If the challenge step surfaced something that genuinely changes what should be built, ask before proceeding rather than unilaterally substituting your reframing for their request.

## Judgment calls

- If the original prompt is already tight and well-specified, say so briefly and proceed — don't manufacture changes for their own sake.
- If the prompt is ambiguous about *what it's for* (e.g. is this going into an API call, a sub-agent, or is Claude just supposed to answer it here), assume "answer it here" unless context says otherwise.
- Keep the rewritten prompt proportional — a one-line ask doesn't need a 200-word rewrite with XML tags.
