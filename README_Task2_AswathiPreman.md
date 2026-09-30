# Task 2 – n8n API Integration Workflow

**Author:** Aswathi Preman

## Overview
This workflow runs on a scheduled trigger, retrieves public Hacker News data, keeps the top five stories, enriches each story with a second Hacker News API request, classifies the stories using a score threshold, and appends the transformed results to Google Sheets.

## APIs / Services Used

1. **Hacker News Firebase API**
   - `https://hacker-news.firebaseio.com/v0/topstories.json`
   - Used to retrieve the current list of top Hacker News story IDs.

2. **Hacker News Item API**
   - `https://hacker-news.firebaseio.com/v0/item/{storyId}.json`
   - Used to enrich the selected story IDs with title, score, author, and URL details.

3. **Google Sheets**
   - Used as the final output destination.
   - The workflow appends Status, Title, Score, Author, and URL as rows.

## Transformation Logic
The first Code node receives the Hacker News story IDs and keeps only the first five.

The second HTTP Request fetches the details for each selected story.

The IF node checks the story score against a threshold of **100**:
- Score > 100 → **High Score**
- Score ≤ 100 → **Normal Score**

Separate Code nodes add the corresponding status and reshape the fields into:
`Status, Title, Score, Author, URL`.

The Merge node combines both branches before the final Google Sheets append operation.

## Error Handling
Both HTTP Request nodes are configured with **Continue using error output**. This prevents an API failure from silently crashing the workflow and exposes the failed request through the node's error output for handling/routing. The normal successful path continues through the transformation and Google Sheets output.

## Result
The workflow was successfully executed and produced transformed rows in Google Sheets.
