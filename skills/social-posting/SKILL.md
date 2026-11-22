---
name: social-posting
description: Draft, schedule, publish and review social media posts with the Minopa MCP server. Use when the user asks to post, schedule or plan content on Instagram, TikTok, Threads, Facebook Pages, X, LinkedIn, Pinterest or YouTube, to upload media for a post, or to see how posts performed.
---

# Posting to social media with Minopa

Minopa works on one workspace: the one the user picked when they signed in. All posts and media belong to that workspace and to the social accounts connected to it in the Minopa app.

## Create a post

1. Call `list_social_accounts` to get the connected accounts. If more than one account matches the platform the user asked for, ask which one to use. Accounts with status `error` can't publish until the user reconnects them in Minopa.
2. Call `get_post_fields` with the chosen account IDs. It returns the exact fields each platform accepts for those accounts (post types, caption limits, media counts, TikTok privacy levels, YouTube categories). Build the post from this schema.
3. If the post needs an image or video:
   - Call `get_media_specs` for the platform and post type before you create or pick media.
   - Upload a public file with `upload_media_from_url`, or pick an existing item with `list_media`.
   - Call `get_media` until `processingStatus` is `processed`.
   - If a file doesn't meet the platform's specs, `convert_media` makes a converted copy (paid plans).
4. Call `create_post` with `workspaceIntegrationIds` and one field object per platform:
   - Leave out `scheduledTime` to save a draft.
   - Set `scheduledTime` (ISO 8601 with the workspace time zone offset, in the future) to schedule.
   - Set `publishNow: true` only when the user clearly asked to publish right away.
   - Send a unique `idempotencyKey` per intended post, so a retry never posts twice.
5. Publishing runs in the background. Call `get_post` until the status is `published` or `rejected`; for a rejected post, tell the user the `statusReason`.

Pinterest posts need a board: get a `boardId` from `list_pinterest_boards` (and a section from `list_pinterest_board_sections` if needed). YouTube posts need a video; a playlist comes from `list_youtube_playlists` and a category from `list_youtube_categories`.

## Change, publish or delete a post

- `update_post` edits a draft or scheduled post. Call `get_post` and `get_post_fields` first. Published posts can't be edited.
- `publish_post` publishes a draft, scheduled or rejected post right away. Confirm with the user first: a published post is public.
- `delete_post` removes the post from Minopa. It doesn't remove content already published on the platform. Confirm with the user first.

## Check results and usage

- `list_posts` and `get_post_stats` show what is drafted, scheduled, published or rejected.
- When the workspace's plan includes analytics, the insight tools (such as `get_analytics_overview`, `get_post_insights`, `list_top_posts` and `get_best_times`) show reach, engagement and the best times to post.
- `get_workspace_usage` and `get_credit_balance` show plan limits. When a call fails with a limit error, check usage and tell the user which limit they hit.

## Errors

Errors come back as JSON with `error` and `code`. On `VALIDATION_ERROR`, fix only the fields named in the details and retry once; don't send the same payload again. `INSUFFICIENT_SCOPES` or `FEATURE_NOT_AVAILABLE` means the user's role or plan doesn't allow the action; tell them instead of retrying.
