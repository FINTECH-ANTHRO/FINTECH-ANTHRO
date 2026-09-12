# FS-2602 — Real Full-Stack Prototype

Browser UI → FastAPI → SQLite + versioned model artifacts.

## Run locally

Windows:

```bat
run.bat
```

macOS/Linux:

```bash
./run.sh
```

Open `http://127.0.0.1:8000/`.

## Architecture

- `frontend/` — the existing Credit Decision Register interface, served by FastAPI.
- `backend/app.py` — API, Scorer, Rules Engine, decision pipeline, replay, fairness, watchlist, audit and H+8 controls.
- `data/fs2602.db` — SQLite database.
- `models/model_artifact.json` — pinned LogisticRegression artifact and metadata.

## Important demo controls

`/decide` is fail-closed. Successful decisions create immutable `decision_snapshots` rows and a hash-chained audit entry. Rulebook publication does not require a redeploy. Replay loads the exact pinned model and rulebook versions from the snapshot. The audit verifier and tamper-test endpoints operate on scratch copies for the live demo; the production tables themselves have database triggers rejecting UPDATE/DELETE.

The model and fairness/watchlist datasets are synthetic and the application is a competition/demo prototype, not a production lending system. Docker/Render uses a persistent data directory and seeds the six demo decisions only when the database has no decision snapshots.
