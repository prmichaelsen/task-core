# Task 98: Publish to npm

**Milestone**: Milestone 3 - Core Library Extraction
**Estimated Time**: 30 minutes
**Dependencies**: Task 97 (Prepare for Publishing)
**Status**: Not Started

---

## Objective

Publish the `@prmichaelsen/task-core` package to npm registry.

## Prerequisites

- [ ] npm account created
- [ ] npm login completed (`npm login`)
- [ ] Package name available on npm
- [ ] All tests passing
- [ ] Documentation complete

## Steps

### 1. Final Verification

```bash
npm run clean
npm run build
npm test
```

Ensure everything passes

### 2. Publish to npm

```bash
npm publish --access public
```

(Use `--access public` for scoped packages)

### 3. Verify Publication

```bash
npm view @prmichaelsen/task-core
```

Check that package is published correctly

### 4. Test Installation

In a separate directory:
```bash
npm install @prmichaelsen/task-core
```

Verify it installs correctly

### 5. Tag Release in Git

```bash
git tag v1.0.0
git push origin v1.0.0
```

---

## Verification

- [ ] Package published to npm
- [ ] Package is publicly accessible
- [ ] Installation works from npm
- [ ] Git tag created
- [ ] Version matches across package.json, git tag, and npm

---

**Status**: Not Started
**Next Task**: Task 99 - Update task-mcp Dependencies
