# ListSimulationJobs<a name="API_ListSimulationJobs"></a>

Returns a list of simulation jobs\. You can optionally provide filters to retrieve specific simulation jobs\. 

## Request Syntax<a name="API_ListSimulationJobs_RequestSyntax"></a>

```
POST /listSimulationJobs HTTP/1.1
Content-type: application/json

{
   "filters": [ 
      { 
         "name": "string",
         "values": [ "string" ]
      }
   ],
   "maxResults": number,
   "nextToken": "string"
}
```

## URI Request Parameters<a name="API_ListSimulationJobs_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_ListSimulationJobs_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [filters](#API_ListSimulationJobs_RequestSyntax) **   <a name="robomaker-ListSimulationJobs-request-filters"></a>
Optional filters to limit results\.  
The filter names `status` and `simulationApplicationName` and `robotApplicationName` are supported\. When filtering, you must use the complete value of the filtered item\. You can use up to three filters, but they must be for the same named item\. For example, if you are looking for items with the status `Preparing` or the status `Running`\.  
Type: Array of [Filter](API_Filter.md) objects  
Array Members: Fixed number of 1 item\.  
Required: No

 ** [maxResults](#API_ListSimulationJobs_RequestSyntax) **   <a name="robomaker-ListSimulationJobs-request-maxResults"></a>
When this parameter is used, `ListSimulationJobs` only returns `maxResults` results in a single page along with a `nextToken` response element\. The remaining results of the initial request can be seen by sending another `ListSimulationJobs` request with the returned `nextToken` value\. This value can be between 1 and 1000\. If this parameter is not used, then `ListSimulationJobs` returns up to 1000 results and a `nextToken` value if applicable\.   
Type: Integer  
Required: No

 ** [nextToken](#API_ListSimulationJobs_RequestSyntax) **   <a name="robomaker-ListSimulationJobs-request-nextToken"></a>
If the previous paginated request did not return all of the remaining results, the response object's `nextToken` parameter value is set to a token\. To retrieve the next set of results, call `ListSimulationJobs` again and assign that token to the request object's `nextToken` parameter\. If there are no remaining results, the previous response object's NextToken parameter is set to null\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `[a-zA-Z0-9_.\-\/+=]*`   
Required: No

## Response Syntax<a name="API_ListSimulationJobs_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "nextToken": "string",
   "simulationJobSummaries": [ 
      { 
         "arn": "string",
         "dataSourceNames": [ "string" ],
         "lastUpdatedAt": number,
         "name": "string",
         "robotApplicationNames": [ "string" ],
         "simulationApplicationNames": [ "string" ],
         "status": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListSimulationJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [nextToken](#API_ListSimulationJobs_ResponseSyntax) **   <a name="robomaker-ListSimulationJobs-response-nextToken"></a>
If the previous paginated request did not return all of the remaining results, the response object's `nextToken` parameter value is set to a token\. To retrieve the next set of results, call `ListSimulationJobs` again and assign that token to the request object's `nextToken` parameter\. If there are no remaining results, the previous response object's NextToken parameter is set to null\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `[a-zA-Z0-9_.\-\/+=]*` 

 ** [simulationJobSummaries](#API_ListSimulationJobs_ResponseSyntax) **   <a name="robomaker-ListSimulationJobs-response-simulationJobSummaries"></a>
A list of simulation job summaries that meet the criteria of the request\.  
Type: Array of [SimulationJobSummary](API_SimulationJobSummary.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 100 items\.

## Errors<a name="API_ListSimulationJobs_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **InternalServerException**   
AWS RoboMaker experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 **InvalidParameterException**   
A parameter specified in a request is not valid, is unsupported, or cannot be used\. The returned message provides an explanation of the error value\.  
HTTP Status Code: 400

 **ThrottlingException**   
AWS RoboMaker is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 400

## See Also<a name="API_ListSimulationJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/robomaker-2018-06-29/ListSimulationJobs) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/robomaker-2018-06-29/ListSimulationJobs) 