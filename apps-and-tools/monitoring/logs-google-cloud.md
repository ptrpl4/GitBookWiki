# ⛅ GCP Logs

#### links

- [https://cloud.google.com/logging/docs](https://cloud.google.com/logging/docs)

## Syntax

```
expression = ["NOT"] comparison { ("AND" | "OR") ["NOT"] comparison }
```

## Queries

[https://cloud.google.com/logging/docs/view/building-queries](https://cloud.google.com/logging/docs/view/building-queries)

```
"response" AND "successful" AND NOT "error"
```

snippets

```
-- # info filters
-severity=INFO
-- -jsonPayload.request.user_agent=~"PostmanRuntime"
-- -jsonPayload.LEVEL="info"
-- -jsonPayload.LEVEL="warn"
-- -jsonPayload.LEVEL="debug"
-- -jsonPayload.LEVEL="warning"
-- -jsonPayload.LEVEL_NAME="warn"
-- -jsonPayload.LEVEL_NAME="info"

-- # response.status
-- -json_payload.response.status="200"
-- -json_payload.response.status="201"
-- -json_payload.response.status="204"
-- -json_payload.response.status="404"
-- -json_payload.response.status="400"
-- -json_payload.response.status="500"

-- # other filters
-textPayload=~"failed to upload metrics"
-jsonPayload.MESSAGE=~"OpenTelemetry"
-jsonPayload.MESSAGE=~"isForceOTPConfirmation"
```
