# metabase-buildpack (Etison fork)

Heroku buildpack used by [Etison/metabase-deploy](https://github.com/Etison/metabase-deploy) to run Metabase on the `cf-metabase` app. Fork of the dead upstream `metabase/metabase-buildpack`; we maintain it.

- `bin/version` — **the single source of truth for which Metabase version gets deployed**
- `bin/compile` — downloads `https://downloads.metabase.com/v$(cat bin/version)/metabase.jar` at build time (`curl -fsS` so a missing jar fails the build instead of shipping a corrupt one)

Changing `bin/version` does nothing by itself: Heroku fetches this buildpack fresh on each build, so after pushing here you must trigger a rebuild of metabase-deploy (empty commit → `git push heroku master`). Full upgrade procedure and current environment state are documented in metabase-deploy's CLAUDE.md.

Before bumping: verify the jar URL returns 200, and check whether the new Metabase needs a newer Java (`system.properties` in metabase-deploy — 0.63 requires Java 25).
