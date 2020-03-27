# API Contracts
Initially internal, but may be expanded to support external usage when required.

An API contract is a document that is an agreement between different teams for how the API is designed. It contains properties such as  endpoints, request and response details and error handling.  The most common form of an API contract today is an OpenAPI Specification (formerly known as Swagger).

Date: 2020-03-24

## Context
This is a proposal to create and maintain an API Contract(s). The API Contract reflects the public and private interface of ESP. It needs to be current from both a planning and active development perspective.

The requirements for the API Contract were clarified after listening to Devs, Testers and POs; aiming to create a tool for the support of ESP development.

There seem to be various sources of value to be gained by specifying and publishing API contracts for internal use:

|Properties|Example|Who is it valuable to|Why is it valuable|Value|
|---|---|---|---|---|
|API URL|`https://mywebsite.com/`|Dev, PO & Test|Identifying API URL, assuming remains constant|Low|
|Endpoint location & Name|`/tradeLists`|Dev, PO & Test|Identifying API endpoints, assuming changable|High|
|Request JSON|{"key": "value"}|Dev, PO & Test|Knowing details of what to send|High|
|Request Headers|{"header name":"header value"}|Tester & Dev|Knowing details of what to send|High|
|Response JSON|{"key": "value"}|Dev, PO & Test|Knowing details of what to receive|High|
|Response Headers|{"header name":"header value"}|Tester & Dev|Knowing details of what to receive, correlation ID tracking|High|
|System Success responses|`200`, `201`|Dev, PO & Test|Confirmation of successful request from the system|High|
|System Error responses|`400`, `404`, `503`|Dev, PO & Test|Confirmation of unsuccessful request from the system|High|
|System Error JSON responses|`missing counterparty`|Dev, PO & Test|Clarification of unsuccessful request from the system|High|
|Infrastructure Errors responses|`429`|Tester & PO|Why a request was unsuccessful|Low|

|State|Who is it valuable to|Why is it valuable|Value|
|---|---|---|---|
|Planned API changes|Dev, PO & Test|Awareness of planned changes, supports shift left, cross-team planning|High|
|Implementing API changes|Dev, PO & Test|Awareness of ongoing development changes, supports shift left, cross-team integration|High|
|Deployed API changes|Dev, PO & Test|Current state of the product|Low|
|Which APIs are publically and/or internal exposed|Dev, PO & Test||Low|
|API specs of sufficient quality to publish externally|PO||Low|
|Viewable from a browser|Dev, PO & Test|Universally viewable, useful to everybody|High|
|Awareness of API spec changes|Dev, PO & Test|Version control|High|

NB: 
- This is anticipated to outlive the current environments setup.
- Alerts and nofitications will be considered beyond the scope of this implementation.

Getting this information at the right time is also valuable. In general, it is more useful the earlier we get the information.

|State|Branch|Captured|Why is it valuable|Value|
|---|---|---|---|---|
|Planning|Feature|Manually edited created openapi file|pre dev feedback|High|
|In Development|Develop|Generated/Updated openapi file|Immediate feedback|High|
|Deployed|Master|Published openapi file|Mapping the live API|Low|

## Building API: Process Changes
### A Four Pronged Approach

#### Part 1: Usability
With visual user interfaces, we generally understand that the design needs iteration. For a change which affects the UI, we typically go through the process resembling the following:

1. Mock up. 
   - Paper example, non/part functional webpage, etc., whatever is quickest. 
   - For small changes it may be fastest to just do it and show the PO. 
   - For brand new stuff paper is often a good idea.
1. Test the look. 
   - Show it to a Tester, PO, end-user
   - Iterate on the mock up until it is as clear as possible to all parties
1. Implement. 
   - Try to implement the mock up so it works as required.
1. Discover. 
   - During implementation we typically discover facts that weren't obvious on the mock up,i.e;
     - "This box says ContractId and Location but the page is for a contract that can have multiple locations. 
     - Should we change this to a single "Locations"? 
     - Do we need a tab per location? 
     - Should the whole screen be per location?
1. Acceptance. 
   - Once implemented we do another round of usability testing. Typically just the tester and PO.
1. Release.

Currently the definition of APIs only ends up defined at the end of the development cycle. However, the API is an interface and we should consider going through the same process, with all the same remarks. For a brand new API, the following feels like a good process:

1. Mock up. 
   - For small API tweaks, a remark in a case, slack, or a modified JSON file may be appropriate. 
   - For a brand new API, manually creating a swagger document may be best (See Part 2). Whatever is quickest.
1. Test it. 
   - Show it to consuming teams tester, PO and end-user
   - Iterate on the mock.
1. Implement. 
   - Try to implement the mock up so it works as required.
1. Discover. 
   - During implementation we typically discover facts that weren't obvious on the mock up, i.e;
     - "we need this extra field". 
     - "This attribute doesn't make sense at this level". Etc. 
   - In this case, it's probably worth informing the consuming team. 
     - API consumers need to do more work to handle interface changes than human users do.
1. Acceptance. 
   - Once implemented we do another round of usability testing. 
   - Typically just the tester and PO (See Part 3).
1. Release.

#### Part 2: Draft Contracts
It would be useful for teams consuming APIs to know about upcoming new APIs and changes to existing APIs ahead of time. This allows them to prepare for the changes, take them into account during upcoming work and prioritise accordingly. This knowledge may provide opportunities for improvements to the system that would otherwise be unavailable. We should also consider publishing documentation that the consuming team can use to implement required changes early, the more accurate and specific this is, the lower the risk of rework. 

##### Producing a Swagger File
Using a swagger file is very specific, and the team creating the API should prioritise design work first (where this does not increase the overall time taken) in order to increase the accuracy of this information. To do this, hold a meeting involving at least 1 member of the consuming team, and go through your mock up of the API. If one team is ready but the other is not, delay this process until both teams are ready to look at it. If the producing team is ready and the consuming team is not, do not delay the actual work. Hold a design meeting as soon as the consuming team are ready.

##### New API Endpoints
The recommended artifact to produce is a swagger document explaining the change. This allows the consuming team to produce more relible mocks during development as the file is highly specific and supported by tooling. Example requests and responses can also be helpful. This file should be committed in the `/Documentation` folder of the repository, and deleted once a similar document has been generated for the production environment (See Part 3). This process has already been trialed between Condor (producing the API) and Ops Delivery (consuming the API) when creating the Balancing API, which had positive results.  Condor's approach was to first write a stub, use nSwag to generate a swagger file from it, and then tweak it manually in a meeting to improve its quality.  This was then reviewed by the Ops Delivery team.

##### Changes to Existing API Endpoints
The recommendation is to start a conversation with the consuming team during planning phase and find out what they need. Provide a mock up in whatever form seems best to use, and use it as a starting point for the conversation. For very small changes, producing any documentation at all may be overkill. Talk to the consuming team, find out what would be useful to them and how useful it is, and be prepared to try and do the design work required earlier rather than later in the process in order to improve the accuracy of this information.

#### Part 3: Environment Documentation
In order to clearly define what is in each environment, and to make to easier to explore the API surface as a whole, we should have some production documentation. This should be automatically generated from the package when a version is deployed to that environment in order to ensure that it always accurately reflects what is in that environment.
We will add a step to the deployment process to copy the `.json` file for that service to an S3 bucket. There will be a file per environment, per service. This will be held in a centrally accessible location in order to improve explorability (See Part 4).

#### Part 4: Explorability
In order to make the documentation easy to access and read, there will be a website built and maintained by the OpsDelivery team on top of the JSON files in this S3 bucket.

### Proposed Tasks
1. Every team creating a new API or changing one of their APIs should try and kick off the changes with a design phase, involving at least 1 member of the consuming team. This should happen before implementation. If the consuming team are not ready, or do not wish to be involved, they may decline this offer.
1. This member of the consuming team is responsible for communicating any changes within their team, and ensuring the right team members are in the design phase.
1. As an experiment, until we learn more or find a better process, the design phase should produce a swagger document for new APIs. For changes to existing APIs, a more pragmatic approach will be used based on what value it provides to the people involved. In this case, start the meeting, explain the changes somehow, and let the consuming team tell you what would provide value to them.
1. Condor team will need to ensure that every project is building an nSwag document, and this works with the docker build process.
1. Engineering will need to provision an S3 bucket to contain these JSON files, and copy a file to this bucket during the deployment step to a given environment.
1. OpsDelivery will take the JS website built by Scott in ApiSpecGeneration, move it to a new repository, and extend it to display all files for that environment in a drop down.
1. Engineering will need to ensure this website is deployed through CI/CD, permissions, networking, filewalling, IP whitelisting, etc. Lots of work.

### Extension tasks:
1. Tool to combine JSON files per service into a single JSON file for the whole environment.
1. Tool to extend the JSON file produced from the code to include other errors from the infrastructure.
1. Central place to publish "Draft" swagger documents
1. Website on top of the central location for draft documents.
