#/surfreport/{beachID}
Returns the surf height, wind speed, tide levels, water temperature, and general recommendation for whether to go surf for a specified beach.

**NOTE:** {beachID} is the ID for the specified beach. A list of available beaches is at `http://example.com/surfreport/beaches_available`.

##Endpoint
`/surfreport/{beachId}`

##HTTP Method
**`GET`**


##Parameters
Parameter | Data Type | Required | Description
--- | --- | :---: | ---
days | integer | No | Default is 3. Max is 7.
units | string| No | Either `imperial` or `metric`. Default is `imperial`. Imperial: feet, knots, Fahrenheit. Metric: centimeters, kilometers per hour, Celsius.
time | integer | No | Format is unix (ms since 1970). Time zone is UTC.

##Sample Request
>**NOTE:** This sample request is from http://idratherbewriting.com/docapis_doc_sample_requests/.

`https://simple-weather.p.mashape.com/surfreport/123?&days=2&units=metric&hour=1400553000`

	-H 'X-Mashape-Key: APIKey'
	-H 'Accept: application/json'


##Sample Response
>**NOTE:** This sample response is from http://idratherbewriting.com/docapis_doc_sample_responses/.

	{
    "surfreport": [
        {
            "beach": "Santa Cruz",
            "monday": {
                "1pm": {
                    "tide": 5,
                    "wind": 15,
                    "watertemp": 80,
                    "surfheight": 5,
                    "recommendation": "Go surfing!"
                },
                "2pm": {
                    "tide": -1,
                    "wind": 1,
                    "watertemp": 50,
                    "surfheight": 3,
                    "recommendation": "Surfing conditions are okay, not great."
                },
                "3pm": {
                    "tide": -1,
                    "wind": 10,
                    "watertemp": 65,
                    "surfheight": 1,
                    "recommendation": "Not a good day for surfing."
                }
            }
        }
    ]
	}

Item | Description
--- | ---
`beach` | The selected beach. See `http://example.com/surfreport/beaches_available` for a list of beaches.
`{day}` | The selected day of the week for the surf conditions. Response returns a default of 3 days and a maximum of 7 days.
`{day}/{time}` | The time for the surf conditions. If request includes `time` parameter, returns surf conditions only for the specified hour. If requests does not include `time` parameter, returns 3 days of surf conditions, listed by hour.
`{day}/{time}/tide` | The tide level at the specified day and time. Negative numbers mean the tide is going out. Positive numbers mean the tide is coming in. A 0 means the tide is neither going out nor coming in.
`{day}/{time}/wind` | The wind speed at the specified day and time. Returned in either kilometers per hour or knots, depending on the unit specified in the request.
`{day}/{time}/watertemp` | The temperature of the water at the specified day and time. Returned in Fahrenheit or Celsius, depending on the unit specified in the request.
`{day}/{time}/surfheight` | The wave height at the specified day and time. Returned in feet or centimeters, depending on the unit specified in the request.
`{day}/{time}/recommendation` | General recommendation of whether to go surfing at the specified day and time. Three possible recommendations: 1. "Go surfing!", 2. "Surfing conditions OK, not great.", 3. "Not today -- try some other activity." Based on returned values of elements `wind`, `watertemp`, and `surfheight`, with each element scored according to how close it is to ideal for surfing at the  specified day and time.

##Code Sample
>**NOTE:** This code sample is from http://idratherbewriting.com/docapis_doc_code_samples/.

This code sample shows how to use the `/surfreport/{beachId}` endpoint to get surf conditions for a specific beach, including a general recommendation about whether to go surfing.
```html
<!DOCTYPE html>
<head>
<script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
<script>
var settings = {
  "async": true,
  "crossDomain": true,
  "dataType": "json",
  "url": "https://simple-weather.p.mashape.com/surfreport/25",
  "method": "GET",
  "headers": {
    "accept": "application/json",
    "x-mashape-key": "APIKEY"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
  $("#surfheight").append(response.query.results.channel.surf.height);
});
</script>
</head>
<body>
<h2>Surf Height</h2>
<div id="surfheight"></div>
</body>
</html>
```
This code sample:
  * Uses the jQuery `ajax` method.
  * Submits the authorization through the header instead of in the endpoint path.
  * Returns only the surf height parameter.

##Error Codes
Code| Description | Message Displayed
:---: | --- | ---
400 | Invalid request or request cannot be processed | Your request is invalid or cannot be processed. {Error message that explains more about the specific problem.}
401 | Missing or invalid authentication credentials | The authentication credentials are missing or invalid.
403 | Missing or invalid authorization credentials | The authorization credentials are missing or invalid.
404 | Specified beach not found | The beach ID you specified is not currently included in our list of available beaches. The up-to-date list of available beaches is at `http://example.com/surfreport/beaches_available`.
429| Reached rate limit | Rate limit reached. Please try again later.
500| Problem with server | There is an internal problem with the server. Please notify the Surf Report development team.
503 | Service unavailable | Surf Report is down for maintenance. Please try again later.
