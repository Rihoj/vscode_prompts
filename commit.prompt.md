---
description: Review workspace changes and create conventional commits using intelligent splitting
name: commit
argument-hint: "Optional: specify files or leave empty to review all changes"
agent: change-splitter-committer
tools: ['vscode', 'execute', 'read', 'edit', 'search', 'mcp_docker/git_add', 'mcp_docker/git_checkout', 'mcp_docker/git_commit', 'mcp_docker/git_create_branch', 'mcp_docker/git_diff', 'mcp_docker/git_diff_staged', 'mcp_docker/git_diff_unstaged', 'mcp_docker/git_init', 'mcp_docker/git_log', 'mcp_docker/git_reset', 'mcp_docker/git_show', 'mcp_docker/git_status', 'agent', 'memory', 'todo']
---

# Create Commits

## Purpose

Review workspace changes, group related modifications into logical commits, and generate Conventional Commit messages.

## When to Use

- **Feature development** with multiple related changes
- **Bug fixes** needing clear change tracking
- **Refactoring** requiring logical commit splitting
- **Multi-part improvements** affecting different systems
- **Review & commit workflow** where you want structured commits
- **Automated change organization** from larger development sessions

## Hard Rules

- **Never self-delegate** — Do not call `#change-splitter-committer` from this prompt
- **Use direct analysis first** — Use git status/diff tools in this prompt
- **Optional specialist review only** — Call `#code-reviewer` only for high-risk or very large diffs
- **Follow Conventional Commits spec** — `type(scope): description`
- **One logical change per commit** — No mega-commits
- **Keep subject line under 72 characters**
- **Use body text for details** — Explain WHY, not WHAT
- **Never force-push without consent** — Preserve collaboration
- **Validate conventional format** — type must be valid (feat, fix, refactor, docs, test, chore, style, perf, ci)
- **Cap output size** — Max 3 proposed commits per response; summarize the rest

## Instructions

1. **Identify changes**: Review staged/unstaged files in workspace
2. **Group changes**: Create atomic commit groups by feature/fix/refactor boundary
3. **Generate messages**: Ensure each commit follows Conventional Commits
4. **Validate output**: Verify each group is standalone and coherent
5. **Present options**: Let user review before committing
6. **Escalate only when needed**: If diff is very large or risky, optionally call `#code-reviewer`

## Conventional Commits Format

**Strict Format**:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Valid Types**:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code restructuring without feature change
- `perf`: Performance improvement
- `docs`: Documentation changes only
- `style`: Formatting, missing semicolons, etc. (no code change)
- `test`: Test additions or modifications
- `chore`: Dependency updates, tooling, build system
- `ci`: CI/CD pipeline changes

**Subject Line** (50 chars max):
- Imperative mood ("add", "fix", "refactor")
- No period at end
- No capitalization

**Example**:
```
feat(auth): implement JWT token refresh mechanism

Add automatic token refresh before expiry with HTTP-only 
cookie storage. Implement 15-minute access token lifespan 
with 7-day refresh token rotation.

Fixes #123
```

## Output Template

Use this concise 4-section format:

### 1. Change Summary
- Files modified, insertions/deletions, key affected areas

### 2. Proposed Commit Groups
- Up to 3 groups with files and rationale
- If more than 3 groups, provide "Remaining groups (summary)"

### 3. Commit Messages
- Full message for each shown group in Conventional Commit format

### 4. Next Actions
- Exact `git add` and `git commit` commands

## Example Usage

**User Input**: `/commit auth files only`

**Expected Behavior**:
- Analyze only matching files
- Propose compact commit groups
- Output concise commit messages and commands

## Success Criteria

✅ No recursive delegation  
✅ Commits follow Conventional Commits format exactly  
✅ Logical grouping makes sense (one feature/fix per commit)  
✅ Commit messages explain WHY changes were made  
✅ No files missing or duplicated across commits  
✅ Each commit can stand alone (buildable, testable)  
✅ First line is imperative mood and under 50 chars  
✅ Output remains compact for large diffs

## Notes

- Use `git diff` to preview changes before committing
- Commits are atomic: each should build and pass tests independently
- For large changesets, split by folder or feature and run multiple `/commit` sessions
- Conventional Commits enable automated changelog generation and semantic versioning
