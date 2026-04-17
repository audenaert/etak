# Agents reference

Agents are autonomous workers. Unlike skills, which you converse with interactively, agents do longer-running work on their own and return a structured result. You invoke them when you want external context, bulk analysis, or synthesis that would be tedious to drive turn by turn.

The [skills](../skills/) are the interactive tools. Agents feed them.

## Available agents

- [**product-researcher**](product-researcher.md) — autonomous competitive research, market trend analysis, and opportunity-space structural review. The thing you reach for when you need outside evidence before a discovery session.

## How agents fit with skills

A useful pattern:

1. **Before exploring.** Run product-researcher to bring in competitive and market context. Skills that generate options ([explore](../skills/explore.md)) produce better options when grounded in external evidence.
2. **Before critique.** Competitor research often gives critique sessions specific material to work with — "how would \[competitor\] respond to this?" is a more interesting question when you know what \[competitor\] has actually been doing.
3. **Periodically.** Run opportunity-space analysis every few weeks to catch structural issues in the graph before they compound.

Agent findings are input, not output. Take the research into a discovery session and the interactive skills turn it into decisions.
