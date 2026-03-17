## What does this PR do?

<!-- A clear and concise description of what this PR implements or fixes. -->



## Related Issue

<!-- Every PR must be linked to an issue. Replace XX with the issue number. -->

Closes #XX

---

## Type of Change

<!-- Check all that apply -->

- [ ] `feat` — New feature
- [ ] `fix` — Bug fix
- [ ] `docs` — Documentation only
- [ ] `test` — Tests only (no production code change)
- [ ] `chore` — Config, tooling, dependencies
- [ ] `refactor` — Code restructure, no behaviour change

---

## Module(s) Affected

<!-- Check all modules touched by this PR -->

- [ ] `auth`
- [ ] `user`
- [ ] `project`
- [ ] `campaign`
- [ ] `bounty`
- [ ] `submission`
- [ ] `review`
- [ ] `reward`
- [ ] `frontend`
- [ ] `config / devops`
- [ ] `other` — ___________

---

## Acceptance Criteria

<!-- Copy the checklist directly from the GitHub issue and tick each item. -->
<!-- Do not open this PR unless every item below is checked. -->

- [ ] 
- [ ] 
- [ ] 

---

## Testing

<!-- Describe how you tested your changes locally. -->

**Unit tests:**
- [ ] New unit tests written for service logic
- [ ] All existing tests still pass (`npm run test`)

**E2E tests (if applicable):**
- [ ] E2E tests written or updated (`npm run test:e2e`)

**Manual testing steps:**
<!-- Briefly describe what you ran/clicked to verify this works -->

1. 
2. 
3. 

---

## Code Quality Checklist

- [ ] TypeScript strict mode — no `any` types introduced
- [ ] ESLint and Prettier pass (`npm run lint`)
- [ ] No `console.log` left in production code
- [ ] No commented-out code
- [ ] DTOs use `class-validator` decorators
- [ ] New environment variables added to `.env.example`
- [ ] Swagger/OpenAPI annotations added or updated (if controller touched)
- [ ] TypeORM migration created for any schema changes

---

## Screenshots

<!-- If this PR includes frontend changes, add before/after screenshots here. -->
<!-- Delete this section if not applicable. -->

| Before | After |
|--------|-------|
|        |       |


---

## Additional Notes

<!-- Anything the reviewer should know — design decisions, trade-offs, follow-up issues, known limitations. -->
<!-- Delete this section if not applicable. -->