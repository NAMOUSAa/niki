# Workspace

## Overview

pnpm workspace monorepo using TypeScript (Node.js) + Python. Contains an Instagram posting bot for a jeans boutique in Port Harcourt, Nigeria.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Artifacts

### Instagram Bot Dashboard (`artifacts/instagram-bot`)
- **Purpose**: Autonomous Instagram posting bot for PH Jeans boutique (Port Harcourt, Nigeria)
- **Frontend**: React + Vite dashboard at `/bot/` — shows live bot status, schedule, post history, controls
- **Backend**: Python Flask bot server at `/bot-api/` — handles scheduling, image analysis, caption gen, reel creation, Cloudinary uploads, Instagram Graph API posting
- **Bot code**: `artifacts/instagram-bot/bot/` — Python files
- **Posting schedule**: 3x/day at 9am, 1pm, 7pm (±15min jitter, Africa/Lagos timezone)
- **Each cycle**: 1 feed image post + 1 reel = 6 posts/day

### Bot Python Files
- `bot/config.py` — all config + env vars
- `bot/tracker.py` — JSON-based post tracking, prevents reposts
- `bot/scheduler.py` — daily schedule generation with jitter
- `bot/image_analyzer.py` — filename-based color/fit analysis
- `bot/caption_generator.py` — Nigerian Pidgin/English captions
- `bot/reel_maker.py` — image → MP4 video with zoom/pan effects
- `bot/uploader.py` — Cloudinary upload for images & videos
- `bot/instagram_poster.py` — Instagram Graph API (feed + reels)
- `bot/bot_engine.py` — orchestrates a full post cycle
- `bot/server.py` — Flask API server with scheduler loop
- `bot/run.py` — entry point (fixes sys.path)

### Media Directories
- `artifacts/instagram-bot/bot/images/` — drop jeans JPG/PNG/WEBP files here
- `artifacts/instagram-bot/bot/music/` — drop MP3/WAV files here for reel audio

## Required Secrets
- `INSTAGRAM_ACCESS_TOKEN` — Instagram Graph API token
- `INSTAGRAM_ACCOUNT_ID` — Instagram Business Account ID
- `CLOUDINARY_CLOUD_NAME` — Cloudinary cloud name
- `CLOUDINARY_API_KEY` — Cloudinary API key
- `CLOUDINARY_API_SECRET` — Cloudinary API secret

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
