Is basically a cloud based FTP server.
And if improperly configured could lead to information leakage.

We can use tools such as:
- CloudWatch -- Log Events -- Log Streams -- Log Groups (monitoring service)
- CloudTrail - JSON Events -- Event History -- Trails (monitors user actions)

`jq '.[]' bookstore.json` grab an array of all data in x.json file.
Example of one array element.
{
"book_title" : "The lion",
"genre": "young_ware",
"page_count": 218
}

On each array element we can utilize the pipe to specify what we want to return.

`jq '.[]' | .book_title bookstore.json` grab book titles
`jq '.[]' | .genre bookstore.json` grab genre 



```shell
ubuntu@tryhackme:~/wareville_logs$ jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName == "wareville-care4wares")' cloudtrail_log.json 
```

```shell
ubuntu@tryhackme:~/wareville_logs$ jq -r '.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]' cloudtrail_log.json
```

```shell
ubuntu@tryhackme:~/wareville_logs$ jq -r '["Event_Time", "Event_Name", "User_Name", "Bucket_Name", "Key", "Source_IP"],(.Records[] | select(.eventSource == "s3.amazonaws.com" and .requestParameters.bucketName=="wareville-care4wares") | [.eventTime, .eventName, .userIdentity.userName // "N/A",.requestParameters.bucketName // "N/A", .requestParameters.key // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t
```

```shell
ubuntu@tryhackme:~/wareville_logs$ jq -r '["Event_Time", "Event_Source", "Event_Name", "User_Name", "Source_IP"],(.Records[] | select(.userIdentity.userName == "glitch") | [.eventTime, .eventSource, .eventName, .userIdentity.userName // "N/A", .sourceIPAddress // "N/A"]) | @tsv' cloudtrail_log.json | column -t -s $'\t'
```

