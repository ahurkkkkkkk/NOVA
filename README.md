# NOVA

NOVA is a Persian-first Telegram assistant for channel publishing, content workflows, subscriptions and account support

## Live bot

- Telegram bot: [@NovaPls_bot](https://t.me/NovaPls_bot)
- Public Telegram bot ID: **8881657128**
- Main interface language: Persian with right-to-left support

## Interface previews

These pictures show design concepts for the NOVA web workspace and checkout
They are illustrative previews and are not screenshots proving that every screen is live

![Supernova workspace design preview for NOVA](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-option-3-supernova-workspace.png)

![Accessible plan and checkout component states](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-PlanCheckout.png)

![Polaris storefront design preview](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-Polaris-ui-preview.png)

## What customers can do in Telegram

- Start the bot, complete onboarding and use NOVA features allowed by their account and subscription
- Browse administrator-configured plans and review available durations, capabilities and usage limits
- Pay for Telegram digital services with Telegram Stars when that method is enabled
- Submit a bank transfer receipt for review when that payment method is enabled
- View account access, subscription status, limits, payment history and support context
- Pause or resume their own NOVA connections where that control is available
- Connect source channels and destinations for channel-to-channel mirroring
- Create automatic publishing feeds from the source catalogue or supported web, RSS and API sources
- Set delivery intervals, item limits, text length, translation, summary and writing tone
- Set include and exclude terms, advertising signals, urgency rules, source attribution and a custom footer
- Configure comment assistance for a linked discussion, including a prompt, tone and saved channel rules
- Build channel context from history and ask questions using channel memory
- Submit text, links or supported media for one-off processing, translation or summarization
- Use an invite link, review referral activity and request eligible withdrawals through the referral wallet
- Open a support request and follow payment or account issues

## What bot administrators can manage

- Customer access, user status, subscriptions, receipts and payment history
- Plans, plan features, limits, prices and trial settings from the protected bot admin controls
- AI provider connections, model choices, provider priority, fallback and configured source keys
- Feed sources, custom source review, channel connections and destination requirements
- Required membership, channel-admin requirements and global publishing controls
- Bot text, button labels, presentation settings, support details and broadcasts
- Support requests, targeted customer messages, referral contracts and withdrawal requests
- Operational status, audit information and protected backups

Plan and trial values are intended to come from administrator settings rather than fixed storefront copy
The web workspace source also contains plan and pricing controls, but simultaneous updates from both admin panels have not been independently verified end to end

## Web workspace and team backend

The NOVA Python backend also contains workspace APIs for organizations and members, channel setup, brand and writing rules, source research, content verification, drafting, media, approvals, scheduled publishing, channel strategy, analytics, growth, advertiser campaigns and organization billing

Those API capabilities are separate from the Telegram bot menus
An API route in the source does not by itself prove that its web screen is complete, connected or validated in production

## Important limits

- Duplicate screening and channel memory code exist, but they do not guarantee that every repeated story will be stopped
- Uncertain duplicate or suspicious-post decisions are not yet verified as a private approval queue for the administrator of each destination channel
- Trial settings exist in bot administration, but web-only trial activation has not been verified in the audited bot source
- Crypto checkout is not connected to a payment provider and is not available as a completed purchase flow
- The deployed Go scheduler and worker are binaries whose source was not available for review, so their internal behavior is not documented here
- Some web workspace routes and administrative sections still need end-to-end validation

This description is based on the verified live Python source and is careful to separate implemented code from incomplete or unverified behavior

## Public repository

This repository keeps only the README in its tracked source tree
The design preview pictures are attached to the [README previews release](https://github.com/ahurkkkkkkk/NOVA/releases/tag/readme-previews)
No bot source code, web app source code, credentials, customer data or runtime configuration is published here
