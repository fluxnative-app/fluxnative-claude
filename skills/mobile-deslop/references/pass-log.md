# Recorded pass

Load this only when the user asks for a recorded pass: one commit per tier, for a pull request, a reviewer stepping through the changes, or a before-and-after record. Rounds, tiers and rulings stay as in `SKILL.md`; this file only adds commits.

## Before the first fix

- The project is a git repository and `git status` is clean. If it is not, ask the user to commit their own changes first. Never commit a file you did not change for the pass.
- Write down the starting commit (`git rev-parse --short HEAD`): it is the before state.

## One commit per tier

- Fix one tier, run the type check (Step 6), then commit that tier's changes alone, before you start the next tier.
- Tiers in the order of Step 5. No commit for a tier with no finding; never two tiers in one commit.
- A shared cause (a theme value, a row component) is fixed once, in the commit of its finding's tier. A design brief written for the pass goes in the first commit whose fixes use it.
- Commit only what the fixes changed: no screenshots, no build output, no lockfile unless a fix needs a package.
- Subject: `deslop(<tier number>-<tier name>): <IDs>`, the tier name as in Step 5 in lower case, the IDs from `tells.md` in finding order, then optionally ` - ` and a few words. Example: `deslop(4-colour): C2, C6 - one accent for actions`.
- Body: the tier's finding rows in the shape from Step 4, one per line.
- Trailers: `Deslop-Round: <n>` and `Deslop-Screens: <routes>`.
- A later round that fixes the same tier again makes a new commit with the same subject form.

## Report

Add a `Commits:` line: the before and after hashes, then one line per commit with its short hash and subject.
