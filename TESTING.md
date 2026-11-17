# Testing

## Quick check (no Docker, no radio hardware)

Requires Git, Python 3.12+ and [uv](https://docs.astral.sh/uv/).

```
uv sync --group dev
uv run python -m pytest tests/notification tests/radio tests/utils
```

A pass confirms the alert logic (keyword matching, config matching, notification
building), radio metadata parsing and utility helpers behave as expected, using the
recorded sample calls in `tests/data`.

Known failure without credentials: `TestNotification::test_alerts_when_keyword_and_traveltime_match`
calls the live Mapbox API and needs `MAPBOX_API_KEY`.

## Lint and types

```
./make.sh lint     # mypy + ruff check + ruff format --check on app/
```

## Full suite

Needs Docker with Compose and a bash shell.

```
cp .env.testing .env
echo "{}" > config/notifications.json
./make.sh test
```

This starts the Compose stack, waits for the API, then runs pytest, including
`tests/test_e2e.py`. `.env` and `config/notifications.json` are gitignored; keep API
keys and volunteer contact details in those files only.

## Live radio

An SDR is only needed for live traffic, not for tests. trunk-recorder (or SDRTrunk)
records calls and uploads them to this app.
