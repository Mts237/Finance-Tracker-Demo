---
name: deploy-check
description: >
  Run this automatically as the LAST step after any task that creates or modifies website files,
  before reporting the task as complete. Verifies that local changes were actually committed and
  pushed to the remote repository, since Vercel deploys directly from GitHub. Prevents reporting a
  task as "done" or "live" when changes never left the local working tree, which causes
  404: NOT_FOUND errors on the live site. Triggers include finishing any edit to HTML/CSS/JS/website
  files, before saying a task is "deployed" or "live", or whenever asked to verify a deployment.
---

# /deploy-check

## When to use
Run this automatically as the LAST step after any task that 
creates or modifies website files, before reporting the task 
as complete. This prevents 404: NOT_FOUND errors on the live 
Vercel deployment.

## Why this matters
Vercel deploys directly from the GitHub repository. If files 
are edited locally but not committed and pushed, Vercel deploys 
an empty or outdated version of the site, resulting in a 
404: NOT_FOUND error for the user.

## Steps to perform every time, before reporting completion
1. Run `git status` to check for uncommitted changes
2. If uncommitted changes exist, commit them with a clear, 
   descriptive message
3. Run `git push` to push the commit to the remote repository
4. Check the output of the push command for any errors 
   (authentication failures, rejected pushes, merge conflicts)
5. Confirm the repository's root directory contains a valid 
   entry file (index.html or equivalent) after the push
6. Only after confirming steps 1-5 succeeded, report the task 
   as complete

## If the push fails
- Do NOT report the task as complete or successfully deployed
- State the exact error message returned by the failed command
- Identify the likely cause (authentication, merge conflict, 
  network issue, permission issue)
- Attempt to resolve the likely cause and retry the push once
- If it still fails, explicitly tell the user: "The build is 
  complete locally, but it could not be pushed to GitHub. The 
  live site will show a 404: NOT_FOUND error until this is 
  resolved. Error: [exact error message]"

## Never do this
- Never report a task as "done," "deployed," or "live" without 
  having verified with actual command output that the push 
  succeeded
- Never assume a push succeeded without checking the command's 
  returned output
