# Claude Review

## Summary

The request path is doing too much repeated work, but the bigger concern is correctness risk if caching is introduced without a precise invalidation strategy.

## Main Claims

- Repeated decrypt work is expensive, but stale config risk is easy to underestimate
- Caching by `baseDir` alone is unsafe if environment-derived values can change
- A prepared API is safer than broad runtime memoization

## Recommendations

1. Prefer bootstrap-time preparation over request-time caching
2. Treat environment-sensitive defaults as part of the cache identity
3. Add regression tests around key rotation and config changes

## Evidence

- Config is derived from both filesystem location and runtime environment
- A broad cache key would not capture all correctness-sensitive inputs
- Similar systems usually fail at invalidation rather than raw crypto speed

## Risks and Caveats

- Over-optimizing the wrong layer can create a performance illusion
- Security and operational safety matter more than a small micro-benchmark win

## Priority

1. Safer API shape
2. Correctness tests
3. Benchmark before broader caching

## Verdict

Take the safer path first: improve structure and tests, then optimize with evidence.
