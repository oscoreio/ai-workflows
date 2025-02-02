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
    types: [labeled]

jobs:
  generate:
    uses: oscoreio/ai-workflows/.github/workflows/issue-to-pr-using-aider.yml@main
    if: startsWith(github.event.label.name, 'aider')
    with:
      issue-number: ${{ github.event.issue.number }}
    secrets: 
      openai-api-key: ${{ secrets.OPENAI_API_KEY }}

```

### Disclaimer

You’re responsible for AI usage costs.  
Only project contributors can label issues to trigger the workflow, preventing abuse by non-contributors.