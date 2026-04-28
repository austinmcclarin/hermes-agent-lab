# Automating New York Times Top Stories to Telegram with Hermes Agent

I set up and validated an automated New York Times delivery workflow using Hermes Agent. The result is a Telegram message with the latest NYT Top Stories sent on demand and scheduled to run every day at 9:00 AM UTC.

## What was built

The workflow now does two things:

1. Sends the current New York Times Top Stories to a configured Telegram home channel
2. Runs automatically every day at 9:00 UTC through a Hermes cron job

## The problem we found

At first, the NYT stories were fetched successfully, but nothing arrived in Telegram.

The root cause was simple: the stories had only been generated in the CLI session and were never actually sent through the Telegram messaging tool.

There was also a second issue that could have affected the scheduled run:

- Telegram itself was correctly configured and connected
- The Hermes gateway service was running normally
- But the cron job had been restricted to the `web` toolset
- In this environment, the `web` toolset was not properly configured for reliable execution

That meant the manual drafting worked, but actual delivery and cron reliability still needed to be fixed.

## What was verified

I checked the live Hermes environment and confirmed:

- Hermes gateway service was active
- Telegram was configured with a working home channel
- Direct Telegram sends from the current session succeeded
- The New York Times RSS feed was reachable and current

The RSS feed used for the workflow is:

`https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml`

## The fix

To make the scheduled delivery reliable, I updated the cron job to avoid the unavailable `web` toolset.

Instead, the job now uses the `terminal` toolset to fetch and parse the NYT RSS feed directly. This is more dependable in the current environment because it works with `python3` and standard XML parsing rather than depending on external web-search configuration.

The cron job was updated to:

- fetch the NYT Top Stories RSS feed
- extract the top 5 stories
- format a compact Telegram-friendly message
- deliver the result to Telegram automatically

## Cron job details

- Job name: `nyt-morning-summary-telegram`
- Schedule: `0 9 * * *`
- Delivery target: `telegram`
- Toolset: `terminal`

This means the message will be sent every day at 09:00 UTC.

## Validation steps performed

I validated the setup in three ways:

### 1. Direct Telegram test
A live test message was sent successfully to the configured Telegram home channel.

### 2. Manual NYT delivery test
The current New York Times Top Stories were fetched and sent successfully to Telegram.

### 3. Cron execution test
The cron job was triggered manually after the update and completed successfully.

Observed result:

- last status: `ok`
- next scheduled run: `09:00 UTC`

## Why this setup is more reliable

Using the NYT RSS feed directly has a few advantages:

- no browser automation required
- no dependency on optional search provider API keys
- predictable feed structure
- easy formatting for Telegram delivery
- simpler cron execution path

For this use case, RSS is the cleanest and most robust source.

## Example output format

A typical Telegram message looks like this:

> NYT Top Stories right now:
>
> 1. Headline one
> URL
>
> 2. Headline two
> URL
>
> 3. Headline three
> URL
>
> 4. Headline four
> URL
>
> 5. Headline five
> URL
>
> Source: NYT Top Stories RSS

## Final result

The automation is now working as intended:

- Telegram delivery is confirmed
- The scheduled cron job is configured correctly
- The data source is stable
- The 9:00 AM UTC run has been tested and should work reliably

## Takeaway

The main lesson from this setup was that generating content in an agent session is not the same as delivering it. Reliable automation requires validating the full path:

1. source retrieval
2. message formatting
3. delivery target
4. scheduled execution

After fixing the delivery path and switching the cron job to a working toolset, the NYT-to-Telegram pipeline is ready for daily use.
