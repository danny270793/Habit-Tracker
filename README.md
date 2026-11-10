# Habit Tracker

Daily habit tracking. Guest data stays on the device. Signed-in data is stored per user in Supabase.

Flutter **3.47.2** (see [`.tool-versions`](.tool-versions)). Android application ID: `io.github.danny270793.habittracker`. iOS bundle ID: `io.github.danny270793.timetracker`.

## Quick start

```sh
cp .env.example.json .env.json   # then fill in real values
asdf exec flutter pub get
asdf exec flutter gen-l10n
asdf exec flutter run --dart-define-from-file=.env.json
```

## Documentation

- [Run on an emulator or device](docs/getting-started.md)
- [Fill `.env.json`](docs/environment.md)
- [Sync Xcode and publish to the App Store](docs/app-store.md)
- [Bump app version and Flutter SDK](docs/versioning.md)
- [Supabase schema](https://github.com/danny270793/supabase): the `habit_tracker_data` table lives in the shared danny270793/supabase repo

## Database

Table `habit_tracker_data` stores one JSON document per `auth.users` id (`habits` jsonb). Guests use local storage only; after sign-in, guest habits merge into the cloud row.

This repo has the app only. The `habit_tracker_*` schema lives in [danny270793/supabase](https://github.com/danny270793/supabase), the source of truth for migrations. Habit Tracker shares that Supabase project with the other apps. To change the database you need both repos:

```sh
git clone git@github.com:danny270793/Time-Tracker.git
git clone git@github.com:danny270793/supabase.git
```

Create, test, and push migrations from the `supabase` repo. This repo ignores any `supabase/` folder, and `.env.json` holds the project credentials, so never commit it. Do not add the service role key to the Flutter app.

## Agents

See [AGENTS.md](AGENTS.md) (Claude: [CLAUDE.md](CLAUDE.md)).

## Deploy

`./scripts/deploy.sh` builds a release APK and publishes it to Firebase App Distribution. It builds with `.env.json`, so set that up first. Before the first run, copy `scripts/.env.deploy.example` to `scripts/.env.deploy` (gitignored), set `FIREBASE_GROUPS` to the tester group alias, and run `firebase login`. `--bump` raises the build number and `--dry-run` prints the commands without running them.
