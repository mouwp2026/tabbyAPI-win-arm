# Windows ARM64 branch (RTX Spark N1X / GB10)

Fork chain:

```
TabbyAPI (upstream)
    -> vcruz305/tabbyAPI      (Linux DGX Spark recipe fork, snapshot 65b8927)
        -> repo mouwp2026/tabbyAPI-win-arm, branch win-arm64
```

`win-arm64` adds one change over the snapshot:

- `common/logger.py` - on Windows + winloop, when a client disconnects
  mid-stream the SSE task is cancelled while a chunk is in flight; winloop
  raises `Cannot call write() when UVStream is ...` and uvicorn logs a
  multi-page ASGI traceback that looks like a server crash. The filter
  recognises those records (exception-chain walk with message markers) and
  suppresses them, so a routine browser "stop" no longer floods the log.

Serving config, model and PQ n-gram table setup live in the companion recipe
repo (see README there). Weights are NOT part of this repo: designed for
[turboderp/Qwen3.8-Flash-Next-exl3](https://huggingface.co/turboderp/Qwen3.8-Flash-Next-exl3)
branch `3.05bpw_h5_ng5` with the ExLlamaV3 win-arm64 fork and a
PQ-compressed n-gram table.
