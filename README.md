## action-repo (Webhook trigger repo)

This repository exists only to **trigger GitHub Webhooks** (not GitHub Actions) for the companion project `webhook-repo`.

### What this repo is for
- **push** events: push commits to any branch.
- **pull_request** events: open / update PRs.
- **(bonus) merged PR** events: merge a PR (GitHub still sends `pull_request` with `action=closed` and `pull_request.merged=true`).

`webhook-repo` receives these payloads at its `POST /webhook` endpoint.

### Configure GitHub Webhooks (GitHub UI)
In this repo on GitHub:

1. Go to **Settings → Webhooks → Add webhook**
2. **Payload URL**:
   - Local (ngrok): `https://<your-ngrok-subdomain>.ngrok-free.app/webhook` (run `ngrok http 5001`)
   - Deployed: `https://<your-domain>/webhook`
3. **Content type**: `application/json`
4. **Secret**:
   - Set a strong secret (e.g. `super-long-random-string`)
   - Use the same value in `webhook-repo` as `GITHUB_WEBHOOK_SECRET`
5. **Which events would you like to trigger this webhook?**
   - Select **Let me select individual events**
   - Check:
     - **Pushes**
     - **Pull requests**
6. Click **Add webhook**

### How to trigger events
- **Push**: push to any branch:
  - `git commit --allow-empty -m "test push" && git push`
- **Pull request**: create a PR from a feature branch to `main`.
- **Merge**: merge that PR (the receiver treats it as a `MERGE` event).

