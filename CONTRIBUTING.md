# Contributing to Corgi13

Thank you for your interest in contributing to the Tejo Beauty Enterprise Platform! This document provides guidelines and instructions for contributing.

## Code of Conduct

Be respectful, inclusive, and professional in all interactions with the community.

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch** from `main`
4. **Make your changes** following the guidelines below
5. **Commit and push** to your fork
6. **Create a Pull Request** with a clear description

## Development Setup

```bash
# Clone with submodules
git clone https://github.com/YOUR_USERNAME/corgi13.git
cd corgi13
git submodule update --init --recursive

# Install dependencies
cd tejospec
pnpm install

# Create a feature branch
git checkout -b feature/your-feature-name
```

## Commit Message Format

Follow conventional commits for clear history:

```text
type(scope): description

[optional body]

[optional footer]
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring without feature changes
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `docs`: Documentation changes
- `style`: Code style (formatting, semicolons, etc.)
- `chore`: Dependency updates, build changes

### Examples

```bash
feat(auth): add OAuth2 provider integration
fix(checkout): resolve payment processing race condition
test(e2e): add cart persistence scenario
docs(api): update authentication endpoints
```

## Code Style & Standards

### TypeScript

- Use strict mode (`strict: true`)
- Declare explicit types (avoid `any`)
- Use named imports/exports
- Prefix interfaces with `I` (e.g., `IUser`)

### React/Next.js

- Functional components with hooks
- Extract custom hooks to `hooks/` directories
- Use TypeScript for all components
- Memoize expensive renders (`React.memo`, `useMemo`)
- Add `data-testid` attributes to interactive elements

### Node.js/API

- Use async/await (no callback hell)
- Proper error handling with try/catch
- Validate input with Zod or similar
- Log at appropriate levels (info, warn, error)
- Use dependency injection for testability

### General Rules

- Max line length: 100 characters
- Use 2-space indentation
- No trailing whitespace
- Use const by default, let when necessary
- Avoid console.log in production code

## Testing Requirements

All pull requests must include tests:

### Unit Tests

```bash
pnpm test
```

- Aim for >80% coverage on new code
- Test happy path and error cases
- Mock external dependencies
- Use descriptive test names

### E2E Tests (for UI changes)

```bash
pnpm run test:e2e
```

- Test critical user flows
- Include multiple browsers when applicable
- Use semantic locators (role, label) over CSS
- Add `data-testid` to elements if needed

### Running Tests Locally

```bash
# Run all tests
pnpm run verify

# Run specific test file
pnpm test -- path/to/test.spec.ts

# Run with coverage
pnpm test -- --coverage
```

## Code Review Checklist

Before submitting a PR, ensure:

- [ ] Code follows style guidelines
- [ ] Tests pass locally (`pnpm run verify`)
- [ ] New tests added for new functionality
- [ ] TypeScript type-checks pass (`pnpm run type:check`)
- [ ] Linting passes (`pnpm run lint`)
- [ ] No console.log or debug code left
- [ ] Commit messages follow conventional format
- [ ] PR description explains the changes
- [ ] Related GitHub issues are linked

## Pull Request Process

1. **Update main** before creating PR:
   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Push to your fork**:
   ```bash
   git push -f origin feature/your-feature-name
   ```

3. **Create PR on GitHub**:
   - Use descriptive title
   - Reference related issues: "Fixes #123"
   - Provide clear description of changes
   - Include screenshots/videos for UI changes

4. **Respond to review feedback**:
   - Make requested changes in new commits
   - Don't force-push after review starts
   - Mark conversations as resolved

5. **Wait for approval**:
   - At least one maintainer approval required
   - CI/CD checks must pass
   - No merge conflicts

## Database Changes

If adding database schema changes:

1. Create migration:
   ```bash
   pnpm run db:create-migration <name>
   ```

2. Update `tejospec/database/prisma/schema.prisma`

3. Test migration:
   ```bash
   pnpm run db:migrate
   ```

4. Include migration files in PR

## API Changes

For API endpoint changes:

1. Update OpenAPI spec in `tejospec/openapi/`
2. Update TypeScript interfaces
3. Add/update tests
4. Update API documentation in `docs/`
5. Run API type-checks: `pnpm run type:check`

## Documentation

- Update relevant docs in `docs/` directory
- Add JSDoc comments for public APIs
- Update README if adding new features or scripts
- Include examples for new functionality

## Performance Considerations

- Profile before and after for performance changes
- Use React DevTools Profiler for components
- Check bundle size: `pnpm run build --analyze`
- Test with slow network simulation
- Monitor database query performance

## Security

- Never commit secrets or credentials
- Use environment variables for sensitive data
- Validate and sanitize all user input
- Follow OWASP guidelines
- Report security issues privately to maintainers

## Questions or Need Help?

- Open a GitHub Discussion
- Check existing issues/PRs
- Review documentation in `docs/`
- Ask in pull request comments

## License

By contributing, you agree that your contributions will be licensed under the project's license (Proprietary).

---

Thank you for making Corgi13 better! 🎉
