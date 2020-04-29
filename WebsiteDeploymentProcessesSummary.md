# Website Deployment Processes Summary
This is a summary breakdown of the Website (OpsDel) team deployment processes

## Pipelines

### cimachine-pipeline
- Triggered on:
   - commit to `origin/feature` branch
   - not deployed
   - runs unit (Jest) and local UI browser tests (Cypress)

### develop-pipeline
- Triggered on:
   - merge into `origin/develop` branch
   - runs unit (Jest) and local UI tests (Cypress)
   - deployed to working in progress (WIP) env
   - Integration Checks (Cypress) are run post-deployment
   - no approvals (gateways)
   
### release-pipeline
- Triggered on:
   - moving `origin/master` branch
   - deployed to end-to-end (E2E) env
   - Integration Checks (Cypress) are run post-deployment
   - multiple engineering, tester and PO approvals (gateways)
   - finally deployed into production env
     
## Git
- Branch hierarchy `test` > `feature` > `develop` > `master`
   - `test refactor` branches are considered as `feature` branches
- Tester controls `master` branch
   - essentially what goes into production, given approval gateway checks

## Pull Requests
- `feature` Final Acceptance/Testing PRs
   - Sign off from each role
   - merged in by test
- `test` PRs
   - created from/merged into `feature` branch
   - sign off from each role, where applicable
   - merged in by test
- Test merges in `feature` & `develop` branches
   - assuing appropriate approval
   - kicking off `develop-pipeline`
   - deploying to WIP
- Test moves (usually fast-forward) `master` branch
   - assuing appropriate approval
   - kicking off `release-pipeline`
   - deploying to E2E and Prod

## Tests

### Unit (Jest) tests
- Run when 
   - building (`cimachine-pipeline`)
   - deploying to WIP (`develop-pipeline`)
- Used in conjunction with local UI tests (Cypress)
- designed to test`
  - React components
  - small units of functionality
  - tests which are harder to do in local UI testing
  
### Local UI tests
- Run when 
   - building (`cimachine-pipeline`)
   - deploying to WIP (`develop-pipeline`)
- Used in conjunction with Unit (Jest) tests
- designed to test
  - business website functionality/workflows/rules
  - browser rendering
  - tests which are harder to do in unit testing

### Integration Checks
- Run when 
   - deploying to WIP (`develop-pipeline`)
   - deploying to E2E (`release-pipeline`)
- designed to test
   - system integration of potentially unstable components (WIP)
   - system integration of stable components (E2E)
   - ESP within AWS environment (WIP + E2E)
   - complete functionality happy paths

### Security Testing
- OWASP ZAP
   - automatically run periodically on WIP
   - HUD used when making major changes to pages
   - plans for penetration (pen) testing to be done before we expose externally
   
### Browsers
- currently only support Chrome (latest)
  - all automation run against Chrome (latest)
- fairly confident that React handles Chrome, FF and Edge
- no current consideration outside Chrome, FF & Edge
  - expecting this to be expanded to at least iOS/Safari once decision on mobile interface is made
   
