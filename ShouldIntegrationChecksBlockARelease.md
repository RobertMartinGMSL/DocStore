# Should Integration Checks Block a Release?
https://github.com/GMSLSoft/Gmsl.Esp.Testing/issues/17#issue-568128074

Does not block develop deployment (last, doesnt block master)
Does block master (e2e) pipeline (and following release)

## Comments

### Rob's comments

What if;
1. Tests fail for an error and we are OK going live
   * Due to a more important feature/fix being blocked?
1. Tests fail for a test fault (fix required)
1. Tests fail for a environmental reason that doesnt exist in live

### Emma's comments
1. Tests should be mission critical only
   * Given they are mission critical, they should always pass
   * If we accept failing tests, and they are mission critical, is this an argument they should not be there?
1. Is the cold-start problem inflating the issue?
   * Are failing Integration Checks likely to happen again and frequently? (is this a diminishing occurrence)
1. If failing Integration Checks don't block a release, is there a risk of human error?
   * Forgetting to check and allowing to go live

## Conclusion

### Engineering Questions
1.  Can we set a section if it has a failed components? i.e.,
    * Can we move integration checks to another section between "E2EDeployment" and "ProductionReleaseApproval"?
    * Can this section fail and the pipeline continue to "ProductionReleaseApproval" section?   
1.  Can we set a section to continue if one, and only one, of its components fails? i.e.,
    * Can we set the "ProductionReleaseApproval" to pass if the "E2E-IntegrationCheck" component fails, but not if the "E2ETesterApproval" or "E2ETesterApproval" sections are not passing (approved)?
1. Can we mark failed tests as "seen" (orange?) to confirm they have been dealt with, but are still failing?

### Proposal
Assuming the above is answers we want from Engineering

1. Move Integration Checks into own section, above "ProductionReleaseApproval"
   * Before Tester/PO approval
1. New section will no longer blocks next section (goes orange instead?)
1. Tester/PO approvals now have to consider and agree on failing section to approve
   * Marking as orange ("seen") if possible?
1. Process should be adopted by all teams
