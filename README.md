# Timeblock Buddy

**Current Status: MVP**

Timeblock buddy is an app to help you plan out your day. The end goal is to have a plan that tracks changes. So you can determine how close or far off you were with your plan. This should help you adjust your schedule and estimations over time.

## Tech used
- SvelteKit
- Tailwind
- Supabase

## Getting started
If you want to run Timeblock Buddy yourself, you'll need to setup a new Supabase project. Then add `.env` file to the root of the project:

```
VITE_SUPABASE_URL=<supabase url>
VITE_SUPABASE_KEY=<supabase key>
```

To get the project up and running

```
npm install
npm run dev
```