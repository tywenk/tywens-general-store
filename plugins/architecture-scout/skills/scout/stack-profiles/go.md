# Go Stack Profile

## Import Patterns
- Check for consistent import grouping (stdlib, external, internal)
- Dot imports (`import . "pkg"`) — discouraged, check if present
- Blank imports (`import _ "pkg"`) — side-effect imports, document why
- Check for import path consistency with module path

## Module System
- `go.mod` — check Go version, module path
- `go.sum` — dependency verification
- `vendor/` directory — vendoring strategy
- Check for `replace` directives — local development or fork dependencies

## Framework Signals
- Standard `net/http` vs Gin/Echo/Chi/Fiber — router choice
- gRPC / protobuf — check for generated code patterns
- GORM / sqlx / database/sql — ORM/query builder choice

## Legacy Indicators
- Go version < 1.18 → no generics
- `GOPATH` mode (no go.mod) → very legacy
- `dep` / `glide` / `govendor` files → pre-module dependency management
- `interface{}` vs `any` → pre-1.18
- Manual error wrapping vs `fmt.Errorf` with `%w` → error handling era

## Configuration Surfaces
- `go.mod` / `go.sum`
- `.golangci.yml` — linter configuration
- `Makefile` — build/test/lint commands
- Dockerfile — build patterns (multi-stage)

## Testing Conventions
- `_test.go` file co-location — standard Go pattern
- Table-driven tests — check consistency
- Test helper organization
- `testdata/` directories
- Mock generation (mockgen, counterfeiter, testify)
- Integration test build tags

## Common Migration Patterns
- GOPATH → modules: Look for missing go.mod
- Pre-generics → generics: Look for `interface{}` heavy code
- Monolith → service: Look for `cmd/` directory patterns
- Error handling patterns: sentinel errors vs custom types vs wrapped errors
