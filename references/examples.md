# Supporting Examples

The canonical teaching material is split into [`good-examples.md`](good-examples.md) and [`bad-examples.md`](bad-examples.md). Read those files first.

This file is retained as a supporting index for the earlier conversation-derived material. It does not define additional rules and intentionally contains no competing code examples. If an older conversation example conflicts with `SKILL.md`, the current skill and canonical example files win.

## Use the canonical examples

- [`good-examples.md`](good-examples.md) shows acceptable code shapes.
- [`bad-examples.md`](bad-examples.md) shows reject patterns and explains the correction.
- [`../README.md`](../README.md) explains the standards for a reader who is deciding whether to install the skill.
- [`../SKILL.md`](../SKILL.md) contains the complete agent workflow and compliance gate.

## Example design rules

When adding a new example:

1. Keep the scenario small enough to understand immediately.
2. Change only the rule being demonstrated in a good/bad pair.
3. Keep unrelated documentation, accessibility, syntax, spacing, and compatibility rules correct. Include new DocBlocks or JSDoc only when explicitly requested; documented fixtures must be labeled as existing documentation being maintained or explicitly requested documentation. Do not treat undocumented declarations as reject patterns solely for lacking blocks.
4. Use lowercase kebab-case BEM in generic CSS examples and make corresponding HTML, CSS, and JavaScript names agree.
5. Label a reject example clearly so it cannot be mistaken for code to copy.
6. Treat optional padding inside CSS function and at-rule condition parentheses, and a space between an at-rule keyword and its opening condition parenthesis, as reject patterns. Preserve required or meaningful spaces such as property-colon spacing, spaces between comma-separated values, arithmetic operators, and media-query separators.
