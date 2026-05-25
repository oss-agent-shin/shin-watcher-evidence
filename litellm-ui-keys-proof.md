# LiteLLM UI — keys page proof

This note records that the LiteLLM proxy + UI were started against the
sandbox's pre-provisioned Postgres DB, and the `/ui/?page=api-keys`
view was screenshotted as evidence.

## How it was started

```
$ litellm-status
{"status": "down", "port": null, "error": "no proxy running; run litellm-up"}

$ litellm-up
[start-db] PostgreSQL already accepting connections.
{"port":57679,"master_key":"sk-1234","url":"http://127.0.0.1:57679","status":"ready"}

$ litellm-status   # after ~15s
{"status": "ready", "port": 47079, "error": null}

$ litellm-shot http://127.0.0.1:47079 "?page=api-keys" /home/user/keys.png
$ file /home/user/keys.png
/home/user/keys.png: PNG image data, 1440 x 900, 8-bit/color RGB, non-interlaced
```

The screenshot itself is embedded in the PR body.
