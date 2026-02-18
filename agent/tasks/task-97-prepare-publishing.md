# Task 97: Prepare for Publishing

**Milestone**: Milestone 3 - Core Library Extraction
**Estimated Time**: 1 hour
**Dependencies**: Task 96 (Update Documentation)
**Status**: Not Started

---

## Objective

Prepare the package for publishing to npm registry.

## Steps

### 1. Verify package.json

Check:
- Name: `@prmichaelsen/task-core`
- Version: `1.0.0`
- Description is accurate
- Keywords are relevant
- License is set (MIT)
- Repository URL (if applicable)
- Author information

### 2. Test Local Installation

```bash
npm pack
npm install -g ./prmichaelsen-task-core-1.0.0.tgz
```

Verify the package installs correctly

### 3. Check .npmignore

Ensure unnecessary files are excluded:
- `src/` (only dist/ should be published)
- `__tests__/`
- `.spec.ts` files
- `agent/` directory
- Development files

### 4. Verify prepublishOnly Script

Ensure `prepublishOnly` script runs:
- Clean
- Build
- Test

### 5. Dry Run Publish

```bash
npm publish --dry-run
```

Review what will be published

---

## Verification

- [ ] package.json is correct
- [ ] Local installation works
- [ ] .npmignore excludes dev files
- [ ] prepublishOnly script works
- [ ] Dry run shows correct files

---

**Status**: Not Started
**Next Task**: Task 98 - Publish to npm
