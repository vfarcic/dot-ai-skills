---
name: worktree-prd
description: Create a git worktree for PRD work with a descriptive branch name. Infers PRD from context or asks user.
---

# Create Git Worktree for PRD

Create a git worktree with a descriptive branch name based on the PRD title. This ensures feature branches have human-readable names that describe what the work is about.

## Workflow

### Step 1: Identify the PRD

Try to infer the PRD number from the current conversation. Look for PRD references like "PRD 353", "PRD #353", or "prd-353".

If not found in context, ask the user: "Which PRD should I create a worktree for? (e.g., 353)"

### Step 2: Get the PRD Title

If the PRD content is already in the conversation context, extract the title from there.

Otherwise, read the PRD file. PRD files are in the `prds/` directory with naming pattern `[number]-[slug].md`:
```bash
ls prds/ | grep "^[PRD_NUMBER]-"
```

The title is on the first line in format: `# PRD #[number]: [Title]`

### Step 3: Generate Descriptive Branch Name

Convert the PRD title to a branch-friendly name:
1. Start with `prd-[number]-`
2. Extract the title after the colon (e.g., "Update to Kimi K2.5 Model Support")
3. Convert to lowercase
4. Replace spaces with hyphens
5. Remove special characters except hyphens and dots
6. Keep it concise (truncate if very long)

**Examples:**
- "PRD #353: Update to Kimi K2.5 Model Support" → `prd-353-kimi-k2.5-support`
- "PRD #290: Skills Distribution System" → `prd-290-skills-distribution`
- "PRD #264: GitOps Tool ArgoCD Integration" → `prd-264-gitops-argocd-integration`

### Step 4: Check for Existing Worktree/Branch

Verify the branch and worktree don't already exist:
```bash
git branch --list [branch-name]
git worktree list | grep [branch-name]
```

If they exist, inform the user and ask how to proceed.

### Step 5: Create the Worktree

Create the worktree with the descriptive branch name:
```bash
git worktree add ../dot-ai-[branch-name] -b [branch-name] main
```

### Step 6: Confirm and Guide

Show the user:
1. The worktree path created
2. The branch name
3. Instructions to start working:
   ```
   To continue work on PRD [number]:
   cd ../dot-ai-[branch-name]
   ```

## Guidelines

- **Descriptive names**: Branch names should describe the feature, not just the PRD number
- **Consistent format**: Always prefix worktree directory with `dot-ai-`
- **Base on main**: Always branch from `main` for new feature work
- **Clean names**: Keep branch names concise but descriptive
