# Prompt Patterns

## Patterns

### Rubric grading

Ask the model to score against an explicit rubric, then revise to raise the lowest scores.

### Two-pass

Pass 1: draft. Pass 2: critique against checklist and rewrite.

### Extract-then-generate

First extract facts from source text; then generate answer only from extracted facts.

### Delimiters

Wrap untrusted user content in clear fences:

```text
<<<USER_INPUT>>>
...
<<<END>>>
```

## Anti-patterns

- Contradictory constraints ("be concise" + "include every detail")
- Asking for confidential data the model cannot know
- Huge prompts that bury the actual task
- Evaluating style with no exemplar

## Evaluation loop

1. Freeze a set of 5–10 test inputs
2. Change one prompt factor at a time
3. Keep the shorter prompt when quality ties
