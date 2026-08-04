# Lessons Learned

1. Adversary emulation must begin with authorization, review, rollback, and cleanup.
2. Technique folders should never be executed without selecting one exact test.
3. Defender prevention can be a successful validation result.
4. A missing alert does not immediately prove a detection gap.
5. Collection, ingestion, parsing, detection, and alerting must be tested separately.
6. Discovery utilities need behavioural context before alerting.
7. Command-shell detection improves with path, parent, file, and follow-on context.
8. Encoded PowerShell and other obfuscation methods may require separate rules.
9. Testing an existing rule unchanged produces more credible evidence.
10. Cleanup verification is part of the validation result.
11. Small lab tests should use actual counts, not inflated percentages.
12. A documented miss can be more valuable than unsupported full-coverage claims.
