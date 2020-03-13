# API Contracts Proposal
This is a proposal to create and maintain an API Contract.  The API Contract reflects the public and private interface of ESP. It needs to be current from both a planning and active development perspective.

The requirements for the API Contract were clarified after listening to Testers and POs, aiming to create a tool for the support of ESP development.  The requirements appear in the form of a _shopping list_. 


## Cross- Team Requirements

### Knowledge of Endpoints
1. API location
   * Yes
     * URL e.g., `https://mywebsite.com/`
1. Endpoint location & Name
   * Yes
     * e.g., `/tradeLists`
1. Request & Response JSON from the Endpoint
   * Yes
     * body data e.g., `{"key": "value"}`
1. Handled & Unhandled endpoint error status response
   * Yes, from the API Gateway
     * e.g., statuses 200, 400, 500 ranges, etc.
   * If possible, between each of the services
1. JSON response error details
   * Yes, from the API Gateway
     * e.g., invalid content, regardless of response status code
   * If possible, between each of the services
1. Headers
   * Yes
     * e.g., "customer": "CME"
1. Environment Configuration responses
   * Yes
      * Load Balancer, API Gateway, etc. status responses

### Business & Technical Details
* Business meaning behind the rejection
  * PO's confirmed that inferring from a technical document is not sufficient in all cases
  * Detail of the error status
    * e.g., "missing counterparty"
* Technical information should be provided
    * e.g., Completed/Finished status code for each transaction
      
### Availability
* Should be published per environment
* As much information as early as possible in the development cycle
  * Initial VERBS, JSON responses, etc. when planning the APIs
  * If possible, this could be available before any code is written
    * e.g., when the producing team is planning the endpoint changes
    * This is understood to be subject to change during development
      * This enables parallel development & test (no delays)
    * This assumes planning up-front and before development
  * As a compromise, this could be available after the code is written
    * Only shared when the information is stable
      * This causes linear development & test (some delays)
  * It would be preferable to have multiple API Contracts to highlight the state of development
    * At least separated by pre-and post code creation
      * i.e.,`in planning` and `in development`
    * Potentially separated by concept 
      * i.e., `planning` > `in dev` > `in test` > `live`
    * Potentially, and if deemed necessary, by environment
      * i.e., `planning` > `wip` > `e2e` > `live`
    
### Tracking API changes
* API Contract changes should be dated on the API Contracts UI
* It is anticipates API Contract users will use the the API Contracts UI to check for endpoint changes throughout the development process
   * See Wishlist for notifications in the future
* This published information should remain as unchanged as possible post-planning
  * Although changes during development are inevitable, so minor changes are expected
* It is expected that this information will be kept up to date when changes are made
  * Assuming its automatically published, nothing should be out-of-date
  
### How to update API Contract users
* Any known changes should be *flagged by the author to the user* 
  * i.e., onus on the API producer to inform API Contract users of changes they make
  
### Format of the API Contract
1. Single source for all the APIs
   * If possible, within an existing tool
     * e.g., GitHub? SwaggerUI?
     
### Ownership & Responsibility
1. Producing & Testing the data should be the responsibility of the API producer team
1. Ownership & Responsibility of producing & maintaining the UI needs to be decided
   * This will be a part of the system 
   * It should be given the appropriate time and effort to produce
   * This should be public facing for external clients user
      * and potentially outsourcing companies for the website
    
### Future Wishlist
1. Diagram of future data flow through the system
   * i.e., mapping a trade import across the APIs
   * For new features that have yet to have been developed (planning stage)
1. Notification system for API changes
   * If possible, it would preferable to subscribe to specific API endpoints
   * Note: Notifications were originally identified as a 'must have' by the website team, however at the Clarify stage this was demoted due to the potential setup overhead and the identification of different working patterns cross team. 
     * User only notified on relevant API changes
     * This would reduce noise
     * would move Onus onto user to know about API changes if they chose to opt-out

  
