# Contributing to the Lua Users Foundation

This document serves as contribution guidelines and the points listed should be
followed for contributions to the foundation.

Contributions are expected to follow the [code of conduct](CODE_OF_CONDUCT.md).

## Commit Guidelines

As is somewhat standard practice, the following should be observed:

- The usage of Markdown-formatted lists is allowed
- Paragraphs should be broken with an empty line between
- Further lines after the header should be no more than 72 characters
- Further lines, if required, should contain detailed information on:
 1. Implications of the changes in the commit
 2. Reasons the changes are required

Commits relating to one file should follow these guidelines:

- The header line should be no more than 60 characters
- The header line should contain the filename and a short description of what
applying the patch will do, in the format of "If applied, this commit will..."
- The second line should be blank

Example commit message:

```markdown
CONTRIBUTING.md: Establish guidelines for new commits

Implications of Changes:

- Could possibly make it harder for outsiders to contribute
- Could create bloatware in the repository data

Reasons for Changes:

- Creates a structure which can be used to avoid excessive comments
  during the review process
```

Commits relating to multiple files should follow these guidelines:

- The header line should be no more than 50 characters
- The header line should contain a short description of what applying the patch
will do, in the format of "If applied, this commit will..."
- The second line should be blank
- The third line should begin a list of changed files, with a header along the
lines of "Changed Files:"

Example commit message:

```markdown
Apply changes to files as per guidelines

Changed Files:
- README.md
- CONTRIBUTING.md

Reasons for Changes:
- This is an example message, but anyways: The guidelines changed, so this
  commit adapts the documents to follow the new guidelines as of 2018-03-14.
```


## Document Design Guidelines

Contributions to documents for the repository should follow these guidelines:

- Lines should be no more than 80 characters
- Documents should be written using standard Markdown (try to avoid using
GitHub flavoring)
- Documents should use relative URLs when linking to other documents
- Links should use the format of putting a link at the bottom of the page if a
line breaks 80 characters
