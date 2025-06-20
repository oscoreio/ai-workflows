# AI workflows
Reusable AI workflows that speed up development.

## Issue to PR using [Aider](https://aider.chat/)

Automate simple GitHub tasks with Aider by tagging issues with `aider`.  
AI will generate a PR within a minute for your review, saving you time on coding basic features.

Create `.github/workflows/issue-to-pr-using-aider.yml` with the following content:
```yaml
name: Issue to PR using Aider

on:
  issues:
    types: [opened, labeled]

permissions:
  contents: write
  issues: write
  pull-requests: write

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
      # install-command: 'npm install'
      # autofix-command: 'npm run lint:fix'
      # test-command: 'npm build'
    secrets:
      # You need set one of these keys
      openrouter-api-key: ${{ secrets.OPENROUTER_API_KEY }} # while it allows to use models for free, it still required to rate-limiting you
      openai-api-key: ${{ secrets.OPENAI_API_KEY }}
      anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
      groq-api-key: ${{ secrets.GROQ_API_KEY }}
      gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
      cohere-api-key: ${{ secrets.COHERE_API_KEY }}
      deepseek-api-key: ${{ secrets.DEEPSEEK_API_KEY }}
```
![image](https://github.com/user-attachments/assets/8382f4cb-4870-43ca-bd6b-46bc1a172d69)


## Comment to commit using [Aider](https://aider.chat/)

Allow to interact with Aider in active PR using `@aider` in message. It can answer you or do a new commit.

Create `.github/workflows/comment-to-commit-using-aider.yml` with the following content:
```yaml
name: Comment to Commit using Aider

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

permissions:
  contents: write
  issues: write
  pull-requests: write

jobs:
  generate:
    if: ${{ contains(github.event.comment.body, '@aider') }}
    uses: oscoreio/ai-workflows/.github/workflows/comment-to-commit-using-aider.yml@main
    with:
      comment-id: ${{ github.event.comment.id }}
      pr-url: ${{ github.event.issue.pull_request.url || github.event.pull_request.url }}
      is-review: ${{ github.event_name == 'pull_request_review_comment' }}
      # install-command: 'npm install'
      # autofix-command: 'npm run lint:fix'
      # test-command: 'npm build'
    secrets:
      # You need set one of these keys
      openrouter-api-key: ${{ secrets.OPENROUTER_API_KEY }} # while it allows to use models for free, it still required to rate-limiting you
      openai-api-key: ${{ secrets.OPENAI_API_KEY }}
      anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
      groq-api-key: ${{ secrets.GROQ_API_KEY }}
      gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
      cohere-api-key: ${{ secrets.COHERE_API_KEY }}
      deepseek-api-key: ${{ secrets.DEEPSEEK_API_KEY }}
```

## Auto-labeling

Automatically labels new issues using AI analysis. The workflow:
- Retrieves issue content and available repository labels
- Uses AI CLI to determine appropriate labels
- Applies the suggested labels without manual intervention

Create `.github/workflows/auto-labeling.yml` with the following content:
```yaml
name: Auto-labeling issue
on:
  issues:
    types:
      - opened

permissions:
  issues: write

jobs:
  auto-labeling:
    uses: oscoreio/ai-workflows/.github/workflows/auto-labeling.yml@main
    with:
      issue-number: ${{ github.event.issue.number }}
    secrets:
      # You need set one of these keys
      openrouter-api-key: ${{ secrets.OPENROUTER_API_KEY }} # while it allows to use models for free, it still required to rate-limiting you
      openai-api-key: ${{ secrets.OPENAI_API_KEY }}
      anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
      groq-api-key: ${{ secrets.GROQ_API_KEY }}
      gemini-api-key: ${{ secrets.GEMINI_API_KEY }}
      cohere-api-key: ${{ secrets.COHERE_API_KEY }}
      deepseek-api-key: ${{ secrets.DEEPSEEK_API_KEY }}
```

## Model and reasoning effort selection

You can control AI model selection and reasoning effort using GitHub labels:
- Add label starting with `aider-` to select model (e.g. `aider-o3-mini`)
- Use suffix `-high` for high reasoning effort (e.g. `aider-o3-mini-high`)
- Special label `aider-r1-free` uses `openrouter/deepseek/deepseek-r1:free`

## All workflows requires access to LLM providers

You need to get API key from one of providers and set it in `settings/secrets/actions` of your repo or on organization level:  
OPENAI_API_KEY - https://platform.openai.com/api-keys  
OPENROUTER_API_KEY - https://openrouter.ai/settings/keys  
ANTHROPIC_API_KEY - https://console.anthropic.com/settings/keys  
GROQ_API_KEY - https://console.groq.com/keys  
GEMINI_API_KEY - https://aistudio.google.com/app/apikey  
COHERE_API_KEY - https://dashboard.cohere.com/api-keys  
DEEPSEEK_API_KEY - https://platform.deepseek.com/api_keys  

### Disclaimer

You're responsible for AI usage costs.  
Only project contributors can label issues to trigger the workflow, preventing abuse by non-contributors.