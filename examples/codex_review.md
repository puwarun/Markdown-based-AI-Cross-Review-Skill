# Codex Review

## Summary

The largest latency source appears to be repeated RSA decryption inside the request path. The code should move toward a prepared decryptor initialized at process startup.

## Main Claims

- `crypto.privateDecrypt()` is the dominant bottleneck under steady traffic
- The current one-shot API repeats setup work unnecessarily
- A prepared decryptor can reduce repeated overhead without changing behavior

## Recommendations

1. Introduce a prepared decryptor at bootstrap
2. Keep the current behavior as a fallback path during rollout
3. Add benchmark coverage before considering more aggressive caching

## Evidence

- Benchmark notes show RSA decrypt cost dominating request time
- The hot path calls the decrypt routine on every request
- The code already has a natural bootstrap point for preloading keys

## Risks and Caveats

- Any cache keyed too broadly could reuse stale configuration
- Optimization should not weaken key-rotation behavior

## Priority

1. Prepared decryptor
2. Benchmark verification
3. Optional cache tuning if real traffic justifies it

## Verdict

Optimize the decrypt path first, but keep the change conservative and measurable.
