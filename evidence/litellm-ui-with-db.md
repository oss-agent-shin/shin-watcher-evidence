# Evidence: LiteLLM UI running with Postgres DB

Proof that the agent can boot the LiteLLM proxy + UI against a real Postgres
database inside the e2b sandbox and reach the API Keys page through the real
admin login form.

## How it was captured

1. `litellm-status` → `down` (fresh sandbox).
2. `litellm-up` → picked a free port (`47079`), ran `prisma db push` against the
   pre-provisioned Postgres, blocked on `/health/readiness` until 200, printed
   `{"port":47079,"master_key":"sk-1234","url":"http://127.0.0.1:47079","status":"ready"}`.
3. `litellm-shot http://127.0.0.1:47079 "?page=api-keys" /home/user/keys.png` —
   drove the real `/ui/` login form (admin / `sk-1234`), navigated to
   `/ui/?page=api-keys`, screenshotted the viewport (1440×900) with the frugal
   chromium flags so the 4 GB sandbox doesn't OOM.
4. PNG validated: 1440×900 RGB, 293 unique colors in the header region (antd
   blue `#e6f4ff`, LiteLLM purple `#6366f1`, dark text `#111827`) — not a
   blank/login redirect.
5. Hosted via `sandbox_upload_artifact` (presigned R2 URL, 7-day expiry).

## Screenshot

![LiteLLM API Keys page running against Postgres](https://fa4cdcab1f32b95ca3b53fd36043d691.r2.cloudflarestorage.com/lap-agent-artifacts/artifacts/0897cae0-16fc-43e6-8de9-610a864e7b6b/b3b49cb5-9052-46ba-a7dc-dc536dd0e896/keys.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=c51795376314fb81674c0f26532d0fbe%2F20260525%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20260525T224511Z&X-Amz-Expires=604800&X-Amz-Signature=2ebe991913fce00c15bb682444b1d9ca96043c66393e7ac0067fd9bb59092924&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3D%22keys.png%22&x-amz-checksum-mode=ENABLED&x-id=GetObject)
