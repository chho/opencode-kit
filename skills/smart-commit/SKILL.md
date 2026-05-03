---
name: smart-commit
description: Analyze git diff, group changes into logical commits with conventional commit messages, and commit step by step with user confirmation. Load this when the user wants to commit changes, asks to smart commit, or needs help organizing uncommitted changes into meaningful commits.
---

# Smart Commit

Analyze uncommitted git changes, group them into logical commits, generate conventional commit messages, and execute commits one by one with user confirmation.

## Workflow

Follow these steps in order. Do NOT skip any step.

### Step 1: Gather Information

Run the following commands to understand the current state:

1. `git status` — check for staged and unstaged changes
2. `git diff` — show unstaged changes
3. `git diff --cached` — show staged changes
4. If there are untracked files, read their content to understand what they do

If there are no changes at all, tell the user "No uncommitted changes found" and stop.

### Step 2: Analyze and Group

Analyze all changes and group files into logical commits. Use these grouping principles:

- **Feature coherence**: Files that together implement one feature go in one commit
- **Change type**: Separate bug fixes from features from refactoring from chore
- **Layer separation**: Prefer separating frontend/backend changes if they're independent
- **Test proximity**: Tests for a feature should be in the same commit as the feature
- **Config proximity**: Config changes (tsconfig, eslint, etc.) that support a feature go with that feature; standalone config changes get their own commit

For each group, determine:
- Which files belong to it
- What the logical purpose of the change is
- Whether it's a feature, fix, refactor, chore, etc.

### Step 3: Generate Commit Messages

For each group, generate a commit message following the Conventional Commits template below.

### Step 4: Present Plan to User

Present the full plan as a numbered list. For each proposed commit, show:

```
Commit N:
  Files: file1.ts, file2.ts, file3.ts
  Message: ✨feat: add user authentication flow
```

Then ask the user using the `question` tool:
- "以上是建议的提交分组方案，是否按此方案执行？" with options: "全部确认", "逐个确认", "重新分组", "取消"

### Step 5: Execute Commits

**If user chose "全部确认"**: Execute all commits in order without further confirmation.

**If user chose "逐个确认"**: For each commit group:
1. Show the commit details (files + message)
2. Ask using the `question` tool: "确认提交这一组？" with options: "确认", "跳过", "修改 message", "取消全部"
3. If confirmed: execute `git add <files>` then `git commit -m "<message>"`
4. If "修改 message": ask the user for the new message, then execute
5. If "跳过": skip to next group
6. If "取消全部": stop immediately

**If user chose "重新分组"**: Ask the user how they'd like to regroup, then redo from Step 2.

**If user chose "取消"**: Stop immediately.

### Step 6: Summary

After all commits are done (or stopped), show a summary:
- Number of commits made
- List of commit messages
- Any skipped groups

---

## Commit Message Template

Generate commit messages that follow the Conventional Commits specification. Use fluent English and emojis.

### How to Generate a Commit Message

1. **Determine the commit type** from the analysis of the diff:

   | Type | Emoji | When to use |
   |------|-------|-------------|
   | init | 🎉 | Initial commit |
   | release | 🚀 | Create a new release |
   | style | 🎨 | Improve code formatting |
   | feat | ✨ | Add a new feature |
   | fix | 🐛 | Fix a bug |
   | docs | 📝 | Update documentation |
   | refactor | ♻️ | Refactor code |
   | perf | ⚡ | Improve performance |
   | dx | 🧑‍💻 | Improve developer experience |
   | workflow | 🔨 | Change build process or CI config |
   | types | 🏷️ | Add or update type annotations |
   | wip | 🚧 | Work in progress |
   | test | ✅ | Add or update tests |
   | build | 📦 | Change build system or external dependencies |
   | ci | 👷 | Change CI configuration files and scripts |
   | chore | 🔧 | Other changes that don't modify src or test files |
   | deps | ⬆️ | Update dependencies |

2. **Breaking changes**: If the commit introduces breaking changes, add `!` after the type.

3. **Description**: Write a brief, imperative description that's 15 words or less.

### Format

```
[emoji][type]: [description]
```

### Rules

- Total message must be **less than 50 characters**
- Use **imperative mood** and **present tense** (e.g., "add" not "added" or "adds")
- Use clear, concise English
- The description should complete the sentence: "If applied, this commit will [description]."

### Examples

```
✨feat: add user authentication
🐛fix: resolve login timeout issue
♻️refactor: extract validation logic
📝docs: update API endpoint docs
🔧chore: update eslint configuration
✅test: add unit tests for auth module
⬆️deps: upgrade react to v19
🐛fix!: change authentication API response format
```
