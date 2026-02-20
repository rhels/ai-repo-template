# Integration Tests (Layer 2)

Tests that run against a lab/staging cluster.
Examples: deploy to test namespace, verify pods, health check routes.

Place executable test scripts here. They should:
- Accept a namespace parameter (or use a default)
- Clean up after themselves
- Exit 0 on success, non-zero on failure
