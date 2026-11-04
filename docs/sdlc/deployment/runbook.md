# Runbook

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start health -->
- Backend: `https://a0e963c551ac.local.awsapprunner.local/health`
- Frontend: https://pa0e963c551ac.dshared.amplifyapp.local
<!-- sdlc:auto:end health -->

<!-- sdlc:auto:start deploy -->
Push to `main` to run `.github/workflows/deploy.yml`, or start a deployment from the platform's Deployment tab.
<!-- sdlc:auto:end deploy -->

<!-- sdlc:auto:start rollback -->
Redeploy the last good version from the platform's Deployment tab, or revert the offending commit on `main` — the workflow redeploys.

**Last live version:** 09f4d1d44da4 (`09f4d1d`)
<!-- sdlc:auto:end rollback -->

<!-- sdlc:auto:start recent-failures -->
_No failed deployments recorded._
<!-- sdlc:auto:end recent-failures -->

## Contacts and escalation

_Add on-call contacts here — this section is never overwritten._
