# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project: Tree Care

An urban tree/plant mapping tool. Users tag trees and plants in their
neighborhood on a map and share info about them (species, care needs,
health), building a crowdsourced green map of a city.

### Core features (MVP)

1. **Map view** — interactive city map showing tagged trees/plants as pins,
   clustered when zoomed out.
2. **Tag a tree/plant** — drop a pin (or use device GPS), then add species,
   photo, size, health status, and care needs (watering, pruning, pests).
3. **Detail page** — view a tree's info, photos, care history, and comments.
4. **Care log** — users record care actions ("watered", "mulched",
   "reported damage") with a timestamp.
5. **Accounts** — sign up / log in; contributions are attributed to users.
6. **Search & filter** — by species, health status, or care needed.

### Later ideas

- Species auto-suggest / photo identification
- Neighborhood stats (tree count, canopy coverage, most common species)
- "Needs water" alerts during dry spells
- Moderation: flag incorrect or duplicate entries
- Import open city tree-inventory datasets

## Tech stack (planned — update once scaffolded)

- **Frontend:** Next.js (App Router) + TypeScript + Tailwind CSS
- **Map:** Leaflet (react-leaflet) with OpenStreetMap tiles
- **Backend:** Next.js route handlers (REST API under `/api`)
- **Database:** PostgreSQL + PostGIS for geospatial queries, via Prisma
- **Auth:** NextAuth.js
- **Image storage:** S3-compatible bucket (or local `uploads/` in dev)

## Data model (draft)

- `User` — id, name, email, createdAt
- `Plant` — id, type (tree/shrub/plant), species, commonName, location
  (lat/lng point), photoUrl, heightEstimate, healthStatus
  (healthy/stressed/damaged/dead), careNeeds, createdById, createdAt
- `CareLog` — id, plantId, userId, action, note, createdAt
- `Comment` — id, plantId, userId, body, createdAt

## Directory layout (planned)

```
src/
  app/            # Next.js routes and pages
    api/          # REST endpoints
  components/     # React components (Map, PlantCard, forms)
  lib/            # db client, auth, geo helpers
prisma/           # schema.prisma and migrations
public/           # static assets
```

## Commands (fill in once the project is scaffolded)

- Install: `npm install`
- Dev server: `npm run dev`
- Lint: `npm run lint`
- Test: `npm test`
- DB migrate: `npx prisma migrate dev`

## Conventions

- TypeScript strict mode; no `any` without a comment explaining why.
- Validate all API input (e.g. with zod); never trust client-sent user IDs.
- Store coordinates as PostGIS points; use bounding-box queries for the map
  so only visible pins are fetched.
- Strip EXIF location data from uploaded photos before storing them
  (privacy: don't leak users' home locations).
- Keep components small; map logic lives in `components/map/`.
- Write tests for API routes and data-validation logic.
- Commit messages: short imperative summary line ("Add care log endpoint").
