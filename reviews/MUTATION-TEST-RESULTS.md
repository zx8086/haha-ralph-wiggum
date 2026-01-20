# Mutation Test Results Summary

## Latest Run (2026-01-20)

### Overall Mutation Score: 56.09%

**Previous**: 53.23%
**Current**: 56.09%
**Improvement**: +2.86%

### Test Execution

- **Duration**: ~1 hour
- **Mutants Tested**: 1963 of 2682 total
- **Configuration**: 8 parallel workers, 30s timeout, perTest coverage analysis

### Results Breakdown

| Category | Count |
|----------|-------|
| Killed | 1100 |
| Timeout | 1 |
| Survived | 862 |
| No Coverage | 0 |
| Errors | 719 |

### Top Performing Files (>90% Score)

| File | Mutation Score | Killed | Survived |
|------|----------------|--------|----------|
| `config/config.ts` | 100.00% | 32 | 0 |
| `config/defaults.ts` | 100.00% | 7 | 0 |
| `handlers/tokens.ts` | 100.00% | 27 | 0 |
| `services/jwt.service.ts` | 93.33% | 28 | 2 |
| `handlers/openapi.ts` | 92.86% | 26 | 2 |

### Areas Needing Improvement (<50% Score)

| File | Mutation Score | Killed | Survived |
|------|----------------|--------|----------|
| `services/cache/cache-factory.ts` | 14.29% | 1 | 6 |
| `telemetry/typed-metrics-poc.ts` | 16.67% | 3 | 15 |
| `handlers/metrics.ts` | 27.27% | 6 | 16 |
| `telemetry/redis-instrumentation.ts` | 33.87% | 21 | 41 |
| `telemetry/telemetry-health-monitor.ts` | 38.46% | 35 | 56 |
| `openapi-generator.ts` | 41.06% | 179 | 257 |
| `telemetry/metrics.ts` | 41.16% | 114 | 163 |

### Category Performance

| Category | Mutation Score | Killed | Survived | Errors |
|----------|----------------|--------|----------|--------|
| **adapters** | 79.61% | 121 | 31 | 57 |
| **cache** | 61.11% | 11 | 7 | 42 |
| **config** | 72.95% | 213 | 79 | 74 |
| **handlers** | 72.56% | 119 | 45 | 72 |
| **services** | 68.53% | 134 | 62 | 117 |
| **telemetry** | 45.42% | 317 | 381 | 320 |
| **utils** | 100.00% | 6 | 0 | 13 |

### Key Findings

#### Strengths
1. **Config System**: 72.95% score with excellent test coverage
2. **JWT Service**: 93.33% score, only 2 surviving mutants
3. **Token Handler**: 100% score, all mutants killed
4. **Utils**: 100% score across all response utilities

#### Weaknesses
1. **Telemetry**: 45.42% average, significant room for improvement
   - Metrics collection has many surviving mutants (163)
   - Health monitoring needs better test coverage (56 survivors)
   - Redis instrumentation poorly covered (41 survivors)

2. **Cache Factory**: 14.29% score - needs comprehensive testing
   - Only 1 of 7 mutants killed
   - Critical initialization paths not tested

3. **OpenAPI Generator**: 41.06% score
   - 257 surviving mutants
   - Large file with complex logic

### Reporter Configuration Note

**Important**: HTML and JSON reporters are disabled due to report size (8000+ mutants) causing `RangeError: Invalid string length` during JSON serialization. Results are captured via:
- Console output (`clear-text` reporter)
- Incremental cache for faster subsequent runs

### Workaround Applied: SIO-276

Successfully implemented bundled Bun executable workaround to eliminate ENOEXEC errors when Stryker spawns Bun processes:
- Bundled Bun CLI with `BUN_BE_BUN=1` support
- Wrapper script for Stryker integration
- Absolute path configuration for sandbox execution

### Next Steps to Improve Score

1. **Cache Factory** (14.29% → target 70%+)
   - Add tests for initialization failures
   - Cover configuration change scenarios
   - Test shutdown and cleanup paths

2. **Telemetry** (45.42% → target 65%+)
   - Improve metrics collection test coverage
   - Add edge case tests for health monitoring
   - Cover Redis instrumentation error paths

3. **OpenAPI Generator** (41.06% → target 60%+)
   - Break down into smaller testable units
   - Add tests for schema generation edge cases
   - Cover error handling paths

### Optimization Status

- **Concurrency**: 8 workers (80% CPU utilization on 10-core system)
- **Timeout**: 30s (aggressive failure detection)
- **Coverage Analysis**: perTest (intelligent test selection)
- **Runtime**: ~1 hour for full mutation test suite

### Commands

```bash
# Standard optimized mutation testing
bun run test:mutation:fast

# Incremental (only changed code)
bun run test:mutation:incremental

# Fresh run (clears cache)
bun run test:mutation:fresh

# Dry run (verify configuration)
bun run test:mutation:dry
```

---

**Generated**: 2026-01-20
**Mutation Framework**: Stryker v8
**Test Runner**: Bun v1.3.6 (bundled executable)
**Total Mutants**: 8024
**Tested Mutants**: 1963 (24.5%)
