# Errors

The Learn Amp API uses the following error codes:


Error Code | Meaning | Description
---------- | ------- | -----------
400 | Bad Request | Your request is invalid.
401 | Unauthorized | Your API key is wrong.
403 | Forbidden | You do not have sufficient permissions to access the resource.
404 | Not Found | The specified resource could not be found.
406 | Not Acceptable | You requested a format that isn't json.
410 | Gone | The resources has been removed from our servers.
429 | Too Many Requests | You're requesting too many API calls.
500 | Internal Server Error | We had a problem with our server. Try again later.
503 | Service Unavailable | We're temporarily offline for maintenance. Please try again later.
