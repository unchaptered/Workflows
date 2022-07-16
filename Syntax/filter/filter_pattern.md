# Filter Pattern

Base

- \*: Matches zero or more characters, but does not match the / character. For example, Octo* matches Octocat.
- **: Matches zero or more of any character.
- ?: Matches zero or one of the preceding character.
- +: Matches one or more of the preceding character.
- [] Matches one character listed in the brackets or included in ranges. Ranges can only include a-z, A-Z, and 0-9. For example, the range[0-9a-z] - matches any digit or lowercase letter. For example, [CB]at matches Cat or Bat and [1-2]00 matches 100 and 200.
- !: At the start of a pattern makes it negate previous positive patterns. It has no special meaning if not the first character.

> More [> Click](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#patterns-to-match-branches-and-tags)