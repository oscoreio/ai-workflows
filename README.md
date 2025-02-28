# AI workflows
Reusable AI workflows that speed up development.

## Issue to PR using [Aider](https://aider.chat/)

Automate simple GitHub tasks with Aider by tagging issues with `aider`.  
AI will generate a PR within a minute for your review, saving you time on coding basic features.

### Usage
- Add your `OPENAI_API_KEY` to repository secrets. Get it from [OpenAI](https://platform.openai.com/api-keys).
- Create `.github/workflows/issue-to-pr.yml` with the following content:

```yaml
name: Issue to PR using Aider
on:
  issues:
    types: [opened, labeled]

jobs:
  generate:
    # Run the job if:
    # - The event is "labeled" and the label name starts with "aider", OR
    # - The event is "opened" and one of the issue's labels contains "aider"
    if: >
      (github.event.action == 'labeled' && startsWith(github.event.label.name, 'aider'))
      ||
      (github.event.action == 'opened' && contains(join(github.event.issue.labels.*.name, ' '), 'aider'))
    uses: oscoreio/ai-workflows/.github/workflows/issue-to-pr-using-aider.yml@main
    with:
      issue-number: ${{ github.event.issue.number }}
    secrets:
      openai-api-key: ${{ secrets.OPENAI_API_KEY }}
```

### Disclaimer

You’re responsible for AI usage costs.  
Only project contributors can label issues to trigger the workflow, preventing abuse by non-contributors.