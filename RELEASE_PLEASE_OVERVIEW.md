# Release Please for Monorepo Release Management

## Overview
Release Please is an open-source tool by Google that automates versioning and changelog generation for monorepos using conventional commits. It creates GitHub Pull Requests with version bumps, changelog updates, and release automation.

## Key Features
- **Automated Version Management** - Determines version bumps based on commit messages
- **Independent Component Releases** - Each package/module versions independently
- **Conventional Commits** - Uses commit message prefixes (feat:, fix:, etc.)
- **Multi-Language Support** - Java/Maven, Node.js, Python, Go, Rust, .NET, and more
- **GitHub Integration** - Creates PRs, tags, and GitHub Releases automatically

## Monorepo Configuration

### 1. Define Components
```json
// release-please-config.json
{
  "packages": {
    "packages/api": { "release-type": "maven" },
    "packages/web": { "release-type": "node" },
    "packages/shared": { "release-type": "simple" }
  }
}
```

### 2. Track Versions
```json
// .release-please-manifest.json
{
  "packages/api": "1.2.3",
  "packages/web": "2.0.1",
  "packages/shared": "0.5.0"
}
```

### 3. GitHub Actions Workflow
```yaml
- uses: googleapis/release-please-action@v4
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
```

## Release Process

### 1. Developer Workflow
```bash
git commit -m "feat(api): add new endpoint"
git commit -m "fix(web): resolve UI bug"
git push origin main
```

### 2. Automated PR Creation
- Release Please analyzes commits since last release
- Creates/updates PR with version bumps per component
- Generates CHANGELOG entries from commit messages
- Updates package manifests (pom.xml, package.json, etc.)

### 3. Release Execution
- Merge PR → Creates git tags (e.g., `api-v1.3.0`, `web-v2.0.2`)
- Publishes GitHub Releases with changelogs
- Optional: Trigger deployment pipelines

## Version Bump Rules

| Commit Prefix | Version Change | Example |
|---------------|----------------|---------|
| `feat:` | Minor bump | 1.2.0 → 1.3.0 |
| `fix:` | Patch bump | 1.2.0 → 1.2.1 |
| `feat!:` or `BREAKING CHANGE:` | Major bump | 1.2.0 → 2.0.0 |
| `chore:`, `docs:` | No release | - |

## Benefits

### For Development Teams
- Eliminates manual version management
- Consistent release process across components
- Clear change history via auto-generated changelogs
- Reduces human error in versioning

### For Monorepos
- Independent versioning per package
- Scope-based releases (only changed components)
- Maintains component relationships
- Scales to hundreds of packages

### For DevOps
- Integrates with CI/CD pipelines
- Predictable release tags for deployments
- Audit trail via PR history
- Rollback capability through git tags

## Best Practices

1. **Use Conventional Commits** - Enforce with linters (commitlint)
2. **Scope by Component** - `feat(component-name): description`
3. **Squash Merge PRs** - One commit per feature for cleaner history
4. **Protected Main Branch** - Require PR reviews before merge
5. **Automate Tests** - Run before merging release PRs

## Cost & Licensing
- **100% Free** - Apache 2.0 License
- **Open Source** - Community-maintained by Google
- **No Vendor Lock-in** - Works with any Git hosting
- **Enterprise-Ready** - Used by Google, Microsoft, and Fortune 500s

## Common Use Cases
- Microservices monorepos (API + multiple services)
- Library ecosystems (shared utilities + packages)
- Full-stack applications (frontend + backend + shared)
- Platform products (core + plugins/extensions)

## Example: Our Maven Monorepo

### Structure
```
maven-monorepo-app/
├── security/           (v0.3.0 - shared library)
└── services/           (v0.4.0 - parent)
    ├── service1/
    └── service2/
```

### Workflow in Action
1. Developer commits: `feat(security): add authentication module`
2. Release Please creates PR updating:
   - `security/pom.xml` version to 0.4.0
   - `security/CHANGELOG.md` with feature description
   - `.release-please-manifest.json` tracking
3. Merge PR → Tag `security-v0.4.0` created + GitHub Release published

### Benefits Realized
- ✅ No manual POM version updates
- ✅ Automatic CHANGELOG generation
- ✅ Independent service versioning
- ✅ Clear release history
- ✅ Zero-cost automation

## Resources
- GitHub: https://github.com/googleapis/release-please
- Documentation: https://github.com/googleapis/release-please/blob/main/README.md
- Conventional Commits: https://www.conventionalcommits.org/
