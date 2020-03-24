# API Contracts
Initially internal, but may be expanded to support external usage when required

Date: 2020-03-24

## Context
This is a proposal to create and maintain an API Contract. The API Contract reflects the public and private interface of ESP. It needs to be current from both a planning and active development perspective.

The requirements for the API Contract were clarified after listening to Devs, Testers and POs; aiming to create a tool for the support of ESP development.

There seem to be various sources of value to be gained by specifying and publishing API contracts for internal use:

|Source of Value|Example|Who is it valuable to|Why is it valuable|Total Value|First Option That Solves|
|---|---|---|---|---|---|
|API location (URL)|`https://mywebsite.com/`|All||High|3|
|Endpoint location & Name|`/tradeLists`|All||High|3|
|Request JSON|{"key": "value"}|All||High|3|
|Request Headers|{"header name":"header value"}|Tester & Dev||High|5|
|Response JSON|{"key": "value"}|All||High|3|
|Response Headers|{"header name":"header value"}|Tester & Dev||High|5|
|Success responses|`200`, `201`|All||High|3|
|Errors from web service|`400`, `404`, `503`|All||High|3|
|Errors from infrastructure|`429`|Tester & PO||Medium|5|
|Error JSON from web service|`missing counterparty`|All||High|3|
|Knowledge of API changes after planning completed||All||High|3|
|Knowledge of API changes during implementation||All||High|3|
|Knowledge of API changes after deployment (live)||All||Low|3|
|Which APIs are publically and/or internal exposed||All||Low|5|
|API specs of sufficient quality to publish externally||PO||Low||
|Viewable from a browser||All||High|2|
|Alerts when the spec of an API changes|Email, slack, etc.|Subscribed||Medium||

Getting this information at the right time is also valuable. In general, it is more useful the earlier we get the information.

|State|Time|Reason for value|Total Value|
|---|---|---|---|
|In planning|in issues|pre dev feedback|Medium|
|In dev|On feature branch|Immediate feedback|High|
|In dev|WIP|Assist testing in WIP|High|
|In dev|E2E|Assist testing in E2E|High|
|Live|Prod|Mapping the live API|Low|

## Options

### Retrospectivly generating local to project (1)
Instate nSwag in each project, built in Debug mode. To get the swagger for an API, you need to clone the repository, build the solution, and then open the JSON produced in Swagger UI. This provides the data for a specific service- it tells you the payloads it receives, sends, and error codes the service intentionally sends. It does not provide the common error codes that can be returned in production from other pieces of infrastructure (eg: 429 rejections from API gateway)

### Retrospectivly Pulibshing to S3 bucket (2)
When a build is deployed to an environment from CodePipeline, the open API JSON should be copied to an S3 bucket for that environment named after the service. A simple website should be built on these JSON files, giving you a drop-down of which one to view for each environment. 

### Proactive & Retrospectively Publishing to S3 bucket per API (3)
Update the API Contract(s);
1. After planning is finished
   * Create openapi file
1. During implementation
   * When feature branches are created
   * When feature branches are merged into dev (WIP)
   * When master branch is updated (E2E)

### Proactive & Retrospectively Publishing to S3 bucket for System (4)
All of (3) then, run a tool to combine into single point of information.  This should be run whenever a new JSON file is deployed which combines all the JSON into a single file, describing the entire API surface for that environment.

### API Gateway Information (5)
All of (3) and potentially (4), then additional information from the API Gateway, Load Balancer, etc. should be added to the combined JSON file, describing possible responses from other pieces of infrastructure (e.g. 504 Gateway Timeout and 429 Too Many Requests)

## Considerations

- (1) already exists, but is retrospective only
  * Least work to implement
- (2) is minimal work to add to (1)
  * All the pieces exist
- (3) Requires manual & automated process, but gives most value
  * partly implemented already in (1)
  * partly process change
- All but (1) require more buy-in
- (4) requires somebody to own and maintain

## Decision

- Implement option (3).  This produces value for all parties, but requires the following
  * Team process to create OpenApi file before code is written
  * Project automation to update OpenApi file once code has started
  * Build automation to populate S3 bucket

## Consequences

- We get an internal website containing future, current and live states of the APIs
- We do not get some sources of value. For example, the full URL, or the required Headers to get through API Gateway.
- All services will need to be changed so they generate an nSwag JSON file on build. This is currently disabled because the 'output' directory in nSwag isn't working with our docker build. Task: Work out how to do this. Probably involves a developer and an Engineer (Scott).
- Engineering (Scott) will need to get the build artifact from CodePipeline and copy it unmodified to an S3 bucket when the service is deployed to that environment. We end up with a JSON file per service.
- Someone (a dev team?) should take over maintenance of the website already in `Gmsl.Esp.ApiSpecGeneration` and make it usable. Possibly move to a different repository? `SwaggerViewingWebsite` or something? May need further involvement from others- eg from Engineering to deploy this website everyone can see.
- Start of API Contract to give to customers, including OPs.