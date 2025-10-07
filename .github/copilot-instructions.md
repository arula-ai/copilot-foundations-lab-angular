# Copilot Agent Instructions for `copilot-foundations-lab-angular`

## Project Overview
- **Angular 15+ / TypeScript** app for Copilot refactoring workshop
- Codebase intentionally contains anti-patterns and memory leaks for training
- Main focus: refactor legacy code, improve test coverage, and apply modern Angular practices

## Architecture & Key Files
- `src/app/utils/date-helper.service.ts`: Large, legacy service with memory leaks, direct DOM manipulation, callback hell, and global state mutations. Primary refactor target.
- `src/app/components/order-list/order-list.component.ts`: Large component with business logic, jQuery usage, missing lifecycle hooks, and performance issues. Secondary refactor target.
- `src/app/data/order-repository.service.ts`: Known memory leak source.
- `src/app/shared/legacy-helper.ts`: Contains anti-patterns.
- `tests/utils/date-helper.service.spec.ts`: Minimal test coverage; expand tests here.

## Developer Workflows
- **Install dependencies:** `npm install`
- **Start dev server:** `ng serve` (http://localhost:4200)
- **Run unit tests:** `npm test`
- **Test coverage:** `npm run test:coverage`
- **Lint:** `npm run lint`

## Refactoring Patterns
- Use RxJS operators (e.g., `takeUntil`, `switchMap`) to prevent memory leaks and avoid nested subscriptions
- Replace direct DOM manipulation with Angular `Renderer2`
- Move business logic out of components into services
- Implement `OnDestroy` lifecycle and clean up subscriptions
- Use `ChangeDetectionStrategy.OnPush` for performance
- Replace getters in templates with computed properties or async pipes
- Eliminate all usages of `any` type
- Remove jQuery and use Angular-native solutions

## Testing Patterns
- Use Jasmine/Karma for unit tests
- Mock services with `jasmine.SpyObj` in tests
- Test for memory leak prevention, error handling, and edge cases
- Expand tests in `tests/utils/date-helper.service.spec.ts` and add component tests for `order-list.component.ts`

## Project-Specific Conventions
- Services should be injected via Angular DI, not instantiated directly
- Avoid direct DOM access; use Angular abstractions
- All subscriptions must be cleaned up using `takeUntil` and `OnDestroy`
- Use trackBy functions in `*ngFor` loops
- Avoid business logic in components; move to services

## Integration Points
- No external APIs; all data/services are local
- No state management (NgRx) yet; consider for future improvements

## Example Anti-Patterns to Fix
- Unsubscribed observables:
  ```typescript
  this.service.getData().subscribe(...); // ❌
  // Use takeUntil and OnDestroy
  ```
- Direct DOM manipulation:
  ```typescript
  document.querySelector('.my-element').style.color = 'red'; // ❌
  // Use Renderer2
  ```
- jQuery usage:
  ```typescript
  $('.order-card').fadeIn(500); // ❌
  // Use Angular-native solutions
  ```

## Success Criteria
- 45%+ test coverage
- <1MB bundle size
- <50 ESLint warnings
- No memory leaks
- Modern Angular patterns throughout

---
For more details, see [README.md](../README.md) and comments in target files. If any section is unclear or missing, ask the user for clarification or examples from the codebase.
