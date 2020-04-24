# Troubleshooting an error when uploading a trade to ESP
If you get an error using ESP and want to investigate, you can get all the logs related to the request if you have the correlation ID.
Note that there can be up to a 15min delay for Insights, so using the `Log Groups` can be quicker but unformatted.

## Obtaining the correlation ID when uploading a trade file
This is easiest if you have the Chrome Developer console open when you click the Upload button:

- Open ESP in Chrome.
- Click F12 to open the developer console.
- With the console open, upload the trade file.
- When the error appears, go to the Network tab in the developer console.
- You will see a red error with name tradeLists. Click on it. 
- In the Headers tab of the information that appears, you will find the correlation id at the end of the Response Headers section. E.g. `x-correlation-id: 8b258946-64af-4156-9086-995b6ac57086`.

## Using the correlation ID to view the logs.
Note that Product Owners only have permission to view logs in WIP at present. If your error is in E2E, you'll need a dev to find the logs.

- Log into AWS and select the appropriate role (e.g. `WIP as Product Owner`).
- Go to Cloudwatch and select Logs > Insights from the menu on the left.
- In the first dropdown, select the following services (the names will vary slightly depending on the environment):
    - Clock
    - Configuration
    - Gemini Configuration
    - Messages
    - Positions
    - Trades 
- In the query box, add the following, replacing the correlation id with the one you found above:
```
fields @timestamp, Summary, concat(Detail.HttpMethod, " ", Detail.Url, Detail.StatusCode), @message
| sort @timestamp
| filter CorrelationId like "ca74bea7-eac5-46c7-92f5-33a10993d72d"
```
- Click Run Query.

You will now see all the logs related to your query. The actual error should be somewhere near the bottom of the stack, with `WARN` in the summary column, followed by one or more HTTP errors being passed up the chain of APIs. Click in the first column to expand the log and see the detail.
