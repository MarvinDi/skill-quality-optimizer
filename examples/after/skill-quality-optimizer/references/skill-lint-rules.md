# Five Pitfalls Detailed Check Rules (Example)

This document is a references file example after split, corresponding to the five pitfalls in the article.

## Pitfall 1: description missing trigger conditions

Check description includes both: (1) what the skill does, (2) when to invoke it.

## Pitfall 2: one skill does too much, not split

Line count over 500 means split into SKILL.md (overview+navigation) + references/ (details).

## Pitfall 3: only requirements, no examples

Body must include at least one code block output example.

## Pitfall 4: rules duplicated across files

Skill, project rules, system prompt each define the same thing only once.

## Pitfall 5: not multi-phrasing tested

Same requirement tested with 3 phrasings, all should trigger the same skill.