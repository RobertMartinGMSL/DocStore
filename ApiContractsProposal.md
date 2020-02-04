# API Contracts Proposal

## Brief
Each API creator team could generate an _API Contract_ for their service.  This would define the end point location as well as any request and response details.  This would be updated proactively, as changes are planned and the implementation process updates the end point.  This would not be useful reactively after implementation changes are made for the end point.  This create a parallel process where both teams can develop effeiciently, rather than a linear process where the consumer team is waiting for creator team to finish implementing changes.

## Details
The consumer teams need to test changes to their website/API in isolation.  This ensures features are implemented before integration testing with the rest of the system, the environment and other external dependencies.  In order to achieve this, mocks are used to mock out the end points.  However, assumptions are made on those mocks which are relied upon to test the features of the website/API. 

The original suggestion was for each creator team to create, maintain and update a test/mock service of their API.  However, this would be expensive, cumbersome and restrictive for the creator team.  It would also require extensive knowledge of how that mock is going to be used by each of the cunsumer teams testing against the it.  The real problem was the validity of the assumptions by the consumer teams.  By producing an API Contract, the risk of consumer team assumptions and strain on the creator teams implementation is minimised.

It is resonable to assume that the creator team will want to change the planned API Contract during implementation, rather than restricting themselves to the original implementation plan.  This would still adhear to the proactive (parallel) process, as any changes would need to be _pushed_ to the API Contract first, before implementation was done.  Assuming the API Contract is generated first, it may require a notification system (on the pipeline) to inform the consumer team of any changes to the API.  By implementing communication, this would reduce the creator teams administrative overhead, whilst keeping the consumer teams assumptions as up to date as possible - encouraging a parallel over linear development process.

## Problems

### Live API Contract
1. Creating the API Contract would be a tedious manual process and would should be automatically generated.
   * SwaggerUI or OpenAPI solves this issue
1. The API Contract will only be as valid as it is both up to date and correct given the current and next implementation of the API.
   * It may be worth auto-generating two API Contracts, one retrospective of changes and one proactive of changes