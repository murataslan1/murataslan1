# Automated GitHub Activity Updates

This repository includes an automated system to update the README.md with recent GitHub activity.

## How It Works

### Components

1. **update-activity.js**: A Node.js script that:
   - Fetches recent public GitHub events for user `murataslan1`
   - Filters events from the last 24 hours
   - Formats them as concise Markdown bullet points
   - Updates only the "Recent GitHub Activity" section in README.md
   - Preserves all other README content unchanged

2. **.github/workflows/update-activity.yml**: A GitHub Actions workflow that:
   - Runs once daily at 00:00 UTC (via cron schedule)
   - Can be manually triggered via workflow_dispatch
   - Executes the update script
   - Commits changes if the README was updated

### README Section

The script updates the content between these markers in README.md:

```markdown
## Recent GitHub Activity

<!-- ACTIVITY_START -->
<!-- This section is automatically updated daily with recent GitHub activity -->
<!-- ACTIVITY_END -->
```

### Supported Event Types

The script formats the following GitHub event types:

- **PushEvent**: Commits pushed to repositories
- **PullRequestEvent**: PRs opened, closed, or merged
- **IssuesEvent**: Issues opened, closed, or reopened
- **IssueCommentEvent**: Comments on issues
- **PullRequestReviewEvent**: PR reviews
- **PullRequestReviewCommentEvent**: Comments on PRs
- **CreateEvent**: Branch or tag creation
- **ForkEvent**: Repository forks
- **WatchEvent**: Repository stars
- **ReleaseEvent**: Release publications

### Manual Execution

To manually run the update script:

```bash
node update-activity.js
```

To manually trigger the GitHub Actions workflow:
1. Go to Actions tab in GitHub
2. Select "Update Recent GitHub Activity" workflow
3. Click "Run workflow"

### Configuration

- **Username**: Set in `update-activity.js` (`USERNAME` constant)
- **Schedule**: Set in `.github/workflows/update-activity.yml` (cron expression)
- **Event limit**: Maximum 10 most recent events are shown (configurable in script)

### Rate Limiting

The script gracefully handles GitHub API rate limiting. If the rate limit is reached, it will skip the update without failing. When run via GitHub Actions, the workflow has access to `GITHUB_TOKEN` for higher rate limits.

## Minimal Changes Philosophy

The implementation follows these principles:

- Only modifies the dedicated activity section
- Preserves all existing README structure, headings, badges, and links
- Makes no changes if there's no meaningful activity
- Creates minimal git diffs
- Does not modify any other repository files or settings
