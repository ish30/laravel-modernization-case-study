# Lessons Learned

1. **Framework upgrades are contract migrations.** Routes, payments, stock, customer identity, schema, caching, and deployment all move together.
2. **Historical migrations may not represent production.** A sanitized structure-only baseline can be more honest and reproducible.
3. **Intermediate major-version checkpoints matter.** They reduce uncertainty and isolate compatibility changes.
4. **Payment exclusions must be deliberate.** Callback routes require explicit review while normal checkout remains protected.
5. **Lockfiles are operational controls.** They define the reviewed release dependency graph.
6. **Green CI is necessary but not sufficient.** Staging validates runtime; monitoring validates production behavior.
7. **Cached configuration must be tested.** Direct `env()` access can look harmless in development and fail after `config:cache`.
8. **Rollback planning improves deployment quality.** Defined thresholds support clearer cutover decisions.
9. **Honest incident documentation strengthens a portfolio.** Root-cause analysis and tested correction show engineering maturity.
10. **Confidentiality and technical depth can coexist.** Architecture, decisions, quality gates, and sanitized metrics provide strong proof without exposing client code.
