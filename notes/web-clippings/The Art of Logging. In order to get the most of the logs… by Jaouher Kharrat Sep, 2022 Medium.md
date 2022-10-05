# The Art of Logging. In order to get the most of the logs… | by Jaouher Kharrat | Sep, 2022 | Medium
[The Art of Logging. In order to get the most of the logs… | by Jaouher Kharrat | Sep, 2022 | Medium](https://medium.com/@JaouherK/creating-a-human-and-machine-freindly-logging-format-bb6d4bb01dca) 

 [The Art of Logging. In order to get the most of the logs… | by Jaouher Kharrat | Sep, 2022 | Medium](https://medium.com/@JaouherK/creating-a-human-and-machine-freindly-logging-format-bb6d4bb01dca) 

 Creating a Human and Machine Friendly Logging Format.
-----------------------------------------------------

Historically, logs have been essential for troubleshooting application and infrastructure performance. Nowadays, it is used for business dashboards visualization and performance analysis.

The importance of structuring the data in those log files so that it can be extracted, manipulated, and analyzed efficiently, in addition to being understandable by humans, is quickly moving up the priority list. The rise of (micro)services also gave birth to another challenge: tracing the propagation of the request throughout the system.

![](https://miro.medium.com/max/1400/1*vKKPTh_3u2hLGyOiwwt_9w.jpeg)

Photo by [Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/documents?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

In this article, we will identify the optimal format for structuring our logs that is easy for humans and machines to parse and understand. Next, we will highlight the key info to log in addition to a proposal of data structure. Finally we will try to provide some important notes to keep in mind for your own projects.

Although logs are originally meant to be parsed, processed, and stored by machines, they are actively being read, understood, and diagnosed by humans. Logs are our best indicators to investigate the murder scene caused by our arch enemy: The Bug! 🐛

![](https://miro.medium.com/max/1400/1*lljBOZwEXmN2Z3Cqwu6GfQ.jpeg)

Photo by [Elisa Ventur](https://unsplash.com/@elisa_ventur?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/bug-computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Nothing can be more frustrating and time-consuming than trying to grasp the information lost within a long and unstructured logline. It is imperative to have a meaningful log that a person can easily understand and dig deeper if the content is relevant to him.

```
66.249.65.159 - - \[06/Nov/2014:19:10:38 +0000\] "GET /news/53f8d72920ba2744fe873ebc.html HTTP/1.1" 404 177 "-" "Debian APT-HTTP/1.3 (0.8.16~exp12ubuntu10.16)"
```

Although we are used to the default Nginx format, the above example is still hard to read and process. It is even harder, when it is part of a huge log file extracted in order to reproduce a bug in production, for example.

The advantages of JSON over other data exchange formats, such as XML, becomes very clear as it’s simple for us humans to both read, write, and understand. How? Its structure is a simple syntax of key-value pairs ordered and nested within arrays.

So what does a log message written in JSON look like? The following is the same previous example of the Nginx web server formatted in JSON:

```
{  
"time": "06/May/2022:19:10:38 +0100",  
"remote\_ip": "66.249.65.159",  
"remote\_user": "-",  
"request": "GET /news/53f8d72920ba2744fe873ebc.html HTTP/1.1",  
"response": 404,  
"bytes": 177,  
"referrer": "-",  
"agent": "Debian APT-HTTP/1.3 (0.8.16~exp12ubuntu10.16)"  
}
```

Let’s consider the above log line example again:

```
66.249.65.159 - - \[06/Nov/2014:19:10:38 +0000\] "GET /news/53f8d72920ba2744fe873ebc.html HTTP/1.1" 404 177 "-" "Debian APT-HTTP/1.3 (0.8.16~exp12ubuntu10.16)"
```

In order to make sense of it, we need to:

*   decipher the syntax,
*   write logic to parse the messages and extract the data we need.

Unfortunately, that logic is fragile. In case something changes in the log format (like a developer adds a new field or changes the items order), then the parser will break. I am sure anyone can face or relate to a similar situation. That’s where a structured format such as JSON can help. The key-value pairs make it easy to extract specific values and to filter and search across a data set. If new key-value pairs are added, the software parsing the log messages will just ignore those keys it doesn’t expect, rather than failing completely.

![](https://miro.medium.com/max/1400/1*xjPczWGV1HJMfUSibFYczw.jpeg)

Photo by [Alex Knight](https://unsplash.com/@agk42?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/machine-friendly?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

The benefits of logging in JSON for machines are:

*   It has a structured format, thus facilitating the analyzis of application logs and querying each and every field.
*   Every programming language can parse it.

Usually, we can aggregate our JSON data in a log parsing system (ELK, newRelic, Datadog etc.) that gives us powerful reporting, searching, and insights to our data. These tools make it easier to index some fields, thus solving the issues of tracing requests through (micro)services environement.

Here is a list of info we should include in any proper log message. Some elements could be optional. The (_o_) next to the field name indicates an optional field.

*   **message** **<string>:** this is a _human-readable_ message to describe the situation → easy to read when filtering to have an overview of the content.
*   **level** **<integer>:** This is a numerical presentation for the priority level (more details in the next section). Very useful to sort messages into different buckets of priority or to generate a dashboard with an overview of the system.
*   **level\_name** **<string>:** This is a “string” presentation for the priority level (more details in the next section)
*   **datetime\_iso <DateTime>:** This is an [iso8601 format](https://en.wikipedia.org/wiki/ISO_8601). It is a required field because we need it to correlate it with other events. Although we can use the server’s date-time, this could be misleading because these servers will use their server acquisition time of the logs which could be a few seconds different or even at a different timezone.
*   **correlation\_id** **<string(uuidv4)>:** this is an important field for the microservices environment. We will use the correlation id of the parsed message/request to trace a request in the whole journey between services.
*   (_o_) **hostname <string>:** Useful to identify which machine generated this log. We recommend it in a microservices environment. It could be redundant when the server logs maps already the original host from the docker’s service name.
*   (_o_) **application<string>:** Useful to identify which device or application generated this log.
*   (_o_) **owner\_id** **<string(uuidv4)/null>:** this will report the user id or API key id if available. We can trace which steps a user has done to reproduce his actions.
*   (_o_) **tenant\_id** **<string(uuidv4)/null>:** this will report the tenant id if available. Very helpful for multi-tenant systems
*   (_o_) **tags** **<string\[\]>:** Could be an array of elements. This element contains meta info about a request like the type, used protocol, etc.
*   (_o_) **stacktrace**: **<string/null>** this is used to display the stack trace in a stringified online format if it exists
*   (_o_) **exception**: **<string/null>** this is used to display the exception message if it exists

So what does a log message written in JSON look like?

The following is the proposed logging concept format through a sample log:

![](https://miro.medium.com/max/1400/1*hTLRKrOSpA937OvlyGQ0Cw.png)

Sample message generated using [carbon](https://carbon.now.sh/)