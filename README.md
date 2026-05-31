# farhanabsar-multicategory-blog-autopost-1djx

## Overview

This n8n workflow automates the creation and publication of SEO-friendly blog posts to a WordPress site. It runs weekly, rotating through predefined categories, researching topics from Google News RSS feeds, generating unique content and a featured image using OpenAI, and finally publishing the complete post with meta-data to WordPress.

## Features

- Weekly scheduled execution for content automation.
- Rotates blog categories weekly based on week number of the month.
- Fetches relevant news articles from Google News RSS feeds for topic inspiration.
- Extracts key information (title, link, image URL) from RSS/Atom feeds.
- Generates SEO-friendly, 600-word blog post content using OpenAI's language models.
- Creates custom, photorealistic featured images with a subtle watermark using OpenAI's DALL-E.
- Uploads generated featured images to the WordPress media library.
- Dynamically retrieves WordPress category IDs based on the selected category name.
- Publishes complete blog posts to WordPress, including AI-generated content, images, and meta-data.

## Services Used

- n8n (workflow automation platform)
- WordPress REST API
- Google News RSS feeds
- OpenAI (for large language models and DALL-E image generation)

## Trigger

This workflow is triggered weekly on Wednesday at 11:00 PM by the 'Schedule Trigger' node.

## Prerequisites

- An n8n instance (self-hosted or cloud).
- A WordPress website with the REST API enabled and accessible.
- An OpenAI API key with access to both chat models (e.g., GPT-4o, GPT-3.5-turbo) and image generation (DALL-E).

## Credentials

- OpenAI API Key (named 'OpenAi account' in the workflow).
- WordPress HTTP Basic Authentication (named 'farhanabsar-wp' in the workflow, requiring a username and application password).

## Configuration

1. Create an n8n HTTP Basic Auth credential for your WordPress site, providing a username and an application password with publishing permissions. Name this credential 'farhanabsar-wp'.
2. Create an n8n OpenAI API credential with your API key. Name this credential 'OpenAi account'.
3. Review the 'Code in JavaScript' node: The `if (day === 'Thursday')` condition will prevent the workflow from executing as scheduled on Wednesday. Change it to `if (day === 'Wednesday')` to align with the 'Schedule Trigger' or adjust the trigger day accordingly.
4. Verify the WordPress domain (`https://farhanabsar.com`) in the 'upload banner to wp media', 'get category id', and 'post blog on wp' nodes, updating it to your specific domain.
5. Adjust the `schedule` object in the 'Code in JavaScript' node if you wish to use different categories or a modified weekly rotation.
6. Optionally, modify the AI prompts in the 'Message a model1' and 'Generate an image1' nodes to tailor content style or image generation requirements.
7. Ensure the OpenAI model `gpt-4.1-mini` used in 'Message a model1' is a valid and accessible model via your OpenAI API key, or update it to a standard model like `gpt-4o` or `gpt-3.5-turbo`.

## Usage

1. After completing the configuration, activate the workflow in your n8n instance.
2. The workflow will automatically run weekly on Wednesday at 11:00 PM (after applying the suggested fix to the 'Code in JavaScript' node).
3. Monitor the workflow execution logs in n8n for successful blog post creation or any errors that may occur.
4. Review the newly published posts directly on your WordPress website to ensure content and formatting are as expected.

## Troubleshooting

- **Workflow not producing output:** Check the 'Code in JavaScript' node. The `if (day === 'Thursday')` condition is a logical error given the 'Schedule Trigger' runs on Wednesday. Correct it to `if (day === 'Wednesday')` to allow the workflow to proceed.
- **Blog posts not appearing on WordPress:** Verify that your WordPress HTTP Basic Auth credential ('farhanabsar-wp') is correct and the user has sufficient permissions. Confirm the WordPress API endpoints are accessible. Check n8n execution logs for specific error messages from WordPress API calls.
- **AI content or image generation issues:** Ensure your OpenAI API key is valid, has sufficient quota, and supports the requested models (GPT-4.1-mini/DALL-E). Review the prompts in 'Message a model1' and 'Generate an image1' for any errors or ambiguities.
- **Incorrect category or news source selection:** Inspect the `schedule` object in 'Code in JavaScript' and the `sources` object in 'Code in JavaScript1' to ensure categories and their corresponding RSS URLs are correctly defined.
- **WordPress post attributes (e.g., categories, featured image) are missing or incorrect:** Verify the data mappings in the 'post blog on wp' node, ensuring that the `id` from 'get category id' and 'upload banner to wp media' nodes are correctly passed.

## Security Notes

- Store API keys and WordPress credentials securely using n8n's built-in credential management system. Avoid hardcoding sensitive information directly in nodes.
- Grant the WordPress user for HTTP Basic Auth only the minimum necessary permissions (e.g., `edit_posts`, `upload_files`, `manage_categories`) to reduce the attack surface.
- Be mindful of the content generated by AI. As posts are automatically published, consider implementing a manual review step or publishing to a 'draft' status first if strict content control is required.
- Monitor OpenAI API usage to manage costs and avoid unexpected charges, especially with image generation and higher-tier language models.
