## How to contribute
We'd love to accept your patches and contributions to this project. There are just a few small guidelines you need to follow.

This repository is documentation, not a library. There is no build to run and no artifact to publish, so a contribution is a change to the Markdown.

## Preparing a pull request for review
Before opening a pull request, check that:

- Every claim about the compiler is backed by the current source in the [Compose compiler plugin](https://github.com/JetBrains/kotlin/tree/master/plugins/compose) or the [Compose runtime](https://github.com/androidx/androidx/tree/androidx-main/compose/runtime). Link the file you read if the behavior is not obvious.
- Code snippets match what the compiler actually emits. The plugin's golden test resources under `compiler-hosted/integration-tests/testResources/golden` are the easiest source to verify against.
- The table of contents still matches the headings if you added, removed, or renamed one.
- Links resolve and code blocks carry a language tag.

## Code reviews
All submissions, including submissions by project members, require review. We use GitHub pull requests for this purpose. Consult [GitHub Help](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) for more information on using pull requests.
