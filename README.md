![logo](./media/sh-banners.png)
=========
[![Maintenance](https://img.shields.io/maintenance/yes/2023.svg?style=flat-square)]()
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

# KQL-Coding-Standards
 > This blog is a conceptual idea and attempt to create a baseline for writing KQL code, also known as the Kusto Query Language.
 
## Introduction

The kusto query language or better known as KQL is a fast growing language in the Microsoft community. More and more resources are using KQL as query language and the most well-known resources and endpoints are probably Azure Monitor, Azure Data Explorer, Log Analytics and the Graph API.

Due to the increasing popularity and usage of KQL, it is time to start the conversation about coding standards for this fast growing query language.

Why? Well, if more people follow the same coding style and standards, the more readable the code will be.

## Consistency is key

_0UR M1ND5 C4N D0 4M4Z1NG 7H1NG5!_

One of the key elements of any language is consistency, and for source code, this is no difference. Long practice in many different programming languages has proven that code that is written consistent is easier to read, understand, and maintain.

By using recognizable patterns and standards, we can stay focused on reading or analyzing the code and prevent our minds from being distracted by the different styles that are currently applied in the wild.

Take a look at the previous note: _0UR M1ND5 C4N D0 4M4Z1NG 7H1NG5!_ You probably had no hard time reading this. So what you have just experienced is that our brains work on a based on pattern recognition.

So to make optimal use of this brain feature we might as well apply it to our code.

## Build up of this blog

Throughout this article, I will use samples taken from various sources, including the Microsoft GitHub, and show how to apply the styling guidelines.

For each use case, a code snippet from a real-life example and a ‘new style’ for the same code snippet is shown. In some cases, the explanation contains multiple style changes from this guide are applied for the sake of consistency. At the end of this blog, it should be clear how to apply each style and why.

### 1. Spaces around special characters

The use of spaces is not relevant to the kusto language, but proper use is key for the improving the readability of code.

#### Code Example

```js
let parser=(
    starttime:datetime=datetime(null),
    endtime:datetime=datetime(null),
    eventresult:string='*',
    disabled:bool=false)
{
```

#### New Style

```js
let parser = (
    starttime:datetime   = datetime(null)
    , endtime:datetime   = datetime(null)
    , eventresult:string = '*'
    , disabled:bool      = false
)
{
```

### 2. Whitespaces around Tabular and String operators

Using whitespaces is generally speaking a good practice, but be aware not to use extraneous whitespaces as this is causing more distraction.

#### Code Example

```js
let AllowedDomains = dynamic ( [ "example.com" , "securehats.com" ] ) ;

oneLogin_CL 
| extend account_id    = tostring(parse_json(key_s)[ 0 ].account_id) 
| extend actor_system  = tostring (parse_json(key_s) [0].actor_system ) 
| extend actor_user_id = tostring ( parse_json (key_s)[ 0 ].actor_user_id )
```

#### New Style

```js
let AllowedDomains = dynamic(["example.com", "securehats.com"]);

oneLogin_CL 
| extend account_id    = tostring(parse_json(key_s)[0].account_id) 
| extend actor_system  = tostring(parse_json(key_s)[0].actor_system) 
| extend actor_user_id = tostring(parse_json(key_s)[0].actor_user_id)
```

In the example above the extraneous whitespaces are removed in the following cases.
- Immediately inside the parentheses
- Immediately before a comma
- after the string operator keyword

### 3. Split arrays in multiple lines

Arrays are commonly used to declare values for later use in the code or in a as lookup function. Because the list of values in an array can become very long, it makes them hard to read when placed on one line. Therefor it is a good practice to split the values across multiple lines.

#### Code Example

```js
let BarracudaProcesses = dynamic(["box_Event_eventS", "box_Auth_access", "box_Config_admin", "box_Firewall_Activity"]);
let SeverityLookup = datatable(LogSeverity:string, EventOriginalSeverity:string) [
  0, "emergency", 1, "alert", 2, "critical", "3", "err", 4, "warn", 5, "notice"];
```

#### New Style

```js
let BarracudaProcesses = dynamic([
    "box_Event_eventS"
    , "box_Auth_access"
    , "box_Config_admin"
    , "box_Firewall_Activity"
]);

let Lookup = datatable(LogSeverity:string, EventOriginalSeverity:string) [
    0, "emergency"
    , 1, "alert"
    , 2, "critical"
    , 3, "err"
    , 4, "warn"
    , 5, "notice"
];
```

If the array has less that 3 values it is not necessary to split the array across multiple lines as shown in the ```datatable``` example.

### 4. Start array values with a comma

Okay this is a controversial one that needs more explaining. In generally comma’s are placed at the end of a line.
But by placing the comma at the start of the line, it becomes easier to out comment one or more values in the array without breaking it.

#### Code Example

```js
| extend time_taken = tolong(Parser[2]), 
    c_ip = tostring(Parser[3]), 
    cs_userdn = tostring(Parser[4]), 
    exception_id= tostring(Parser[6]), 
    sc_filter_result = tostring(Parser[7]), 
    cs_categories = split(Parser[8],";"), 
    cs_referrer = tostring(Parser[9]), 
    sc_status = tostring(Parser[10]), 
//  RemainingString1 = Parser[11]
```

#### New Style

```js
| extend 
    time_taken         = tolong(Parser[2])
    , c_ip             = tostring(Parser[3])
    , cs_userdn        = tostring(Parser[4])
    , exception_id     = tostring(Parser[6])
    , sc_filter_result = tostring(Parser[7])
    , cs_categories    = split(Parser[8], ";")
    , cs_referrer      = tostring(Parser[9])
    , sc_status        = tostring(Parser[10])
//  , RemainingString1 = Parser[11]
```

In the first code example the array is broken because by out commenting the last value, the array finishes with a comma.
In the _new style_ this is avoided by moving the comma’s to the start of the line.

### 5. Avoid complex one-liners

Writing one-liners is a common practice often used in programming languages. For the code it is irrelevant if the code is written on one or multiple lines.

Writing good code is all about understanding what the code does. By splitting complex one-liners in multiple lines will make it easier to see what is actually happening.

#### Code Example

```js
| parse-kv DeviceCustomString3 as (Host:string,Referer:string,['User-Agent']:string,Accept:string,['Content-Type']:string,['X-Forwarded-For']:string) with (pair_delimiter=@'\r\n',kv_delimiter=': ')
```

#### New Style

```js
| parse-kv DeviceCustomString3 as (
    Host:string
    , Referer:string
    , ['User-Agent']:string
    , Accept:string
    , ['Content-Type']:string 
    , ['X-Forwarded-For']:string
) 
with (
    pair_delimiter = @'\r\n'
    , kv_delimiter = ': '
)
```

> By splitting the code in multiple lines it becomes more visible what values are extracted and which delimiters are used.

### 6. Avoid trailing spaces

Trailing spaces are frequently the result of future edits where the only change is a space being added or removed. I have seen this in many occasions where a trailing space was added to the code only to do a commit or to start a CI/CD pipeline.

```js
| project-rename..
    TargetDvcHostname = Computer
    , EventOriginalUid=EventOriginId..........
    , EventOriginalType=EventID..
| extend  EventCount=int(1)
        , EventSchemaVersion='0.1.0'
        , ActorUserIdType='SID'
        , TargetUserIdType='SID'
        , EventVendor='Microsoft'..
        , EventStartTime =TimeGenerated...
        , EventEndTime=TimeGenerated...
```

#### New Style

```js
| project-rename 
    TargetDvcHostname   = Computer
    , EventOriginalUid  = EventOriginId
    , EventOriginalType = EventID
| extend  
    EventCount           = int(1)
    , EventSchemaVersion = '0.1.0'
    , EventVendor        = 'Microsoft'
    , EventStartTime     = TimeGenerated
    , EventEndTime       = TimeGenerated
```

In the code example the whitespaces are replaced by dots to make them visible in this code block.

### 7. One True Brace Style

This style is adopted from Kernighan and Ritchie, who used it in their book _The C Programming Language._ It describes that every braceable _statement_ should have the opening brace on the _end of a line_, and the closing brace at the _beginning of a line._

Brackets are more often used then braces in KQL. So to follow the common coding guidelines the advise is to apply this method also to brackets.

#### Code Example
```js
let ActionLookup = datatable(DvcOriginalAction:string, DvcAction:string)
[ "drop", "Drop"
, "forward", "Allow"
, "source-reset", "Reset Source"
, "dest-reset", "Reset Destination"
, "source-dest-reset", "Reset"
, "proxy", "Allow"
, "challenge", "Reset"
, "quarantine", "Reset"
, "drop-and-quarantine", "Drop"
, "allow", "Allow"];
```

#### New Style

```js
let ActionLookup = datatable(DvcOriginalAction:string, DvcAction:string) [
    "drop", "Drop"
    , "forward", "Allow"
    , "source-reset", "Reset Source"
    , "dest-reset", "Reset Destination"
    , "source-dest-reset", "Reset"
    , "proxy", "Allow"
    , "challenge", "Reset"
    , "quarantine", "Reset"
    , "drop-and-quarantine", "Drop"
    , "allow", "Allow"
];
```

> The opening bracket is placed behind the ```datatable``` operator statement.

### 8. Inline Comments

Comments should only be used to clarify what is happening in the code if it isn’t clear by the code logic itself. Try to keep the (inline) comments short and clear. Also don’t go wild on on creating stylish comment blocks with fancy borders etc.

#### Code Example

```js
PostgreSQL_CL | where not(disabled)
    | where RawData has 'disconnection'
    | extend 
    EventVendor    = 'PostgreSQL'
    , EventProduct = 'PostgreSQL'
    , DvcIpAddr = extract(@'\d{1,3}\.\d{1.3}\.\d{1,3}\.\d{1,3}', 1, Computer)
    , TargetUsernameType = 'Simple'
    , TargetUsername = extract(@'user=(.*?)\sdatabase', 1, RawData)
    , SrcIpAddr = extract(@'host=\[?(.*?)\]?', 1, RawData)
    , EventResultDetails = 'Session expired'
    , EventOriginalRestultDetails = 'User session closed'
  // ************************ 
  //      <Aliases> 
  // ************************
  | extend
          User=TargetUsername
        , Dvc=Computer
  // ************************ 
  //      </Aliases> 
  // ************************
    | project-away Computer, ManagementGroupName, RawData, SourceSystem, TenantId
```

#### New Style
```js
PostgreSQL_CL
  | where not(disabled)
  | where RawData has 'disconnection'
  | extend 
    EventVendor           = 'PostgreSQL'
    , EventProduct        = 'PostgreSQL'
    , DvcIpAddr           = extract(@'\d{1,3}\.\d{1.3}\.\d{1,3}\.\d{1,3}', 1, Computer)
    , TargetUsernameType  = 'Simple'
    , TargetUsername      = extract(@'user = (.*?)\sdatabase', 1, RawData)
    , SrcIpAddr           = extract(@'host = \[?(.*?)\]?', 1, RawData)
    , EventResultDetails  = 'Session expired' 
  // add recommended aliases
  | extend
    User  = TargetUsername
    , Dvc = Computer
  | project-away 
    Computer
    , ManagementGroupName
    , RawData
    , SourceSystem
    , TenantId
```

When reading the code above it should already be clear what is done. Although the section for adding the aliases is also straight forward, it is useful to explain why these optional fields are parsed.

### 9. Avoid Function Aliases

An alias can be a simplified or shortened name of a function or statement in KQL. The downside of aliases is that they can become deprecated, resulting in broken code once not supported anymore. Thus increase of lifecycle management.

Try to avoid the use of aliases and if used in existing code, replace the alias with the correct value.

#### Code Example

```js
print pack("Level", "Information", "ProcessID", 1234, "Data", pack("url", "www.bing.com"))

// Preserve Original fields
| extend AdditionalFields = pack(
    'Action',   var_action
    , 'type',   var_unpacked.[0]
    , 'proto',  var_unpacked.[1]
    , 'srcIF',  var_unpacked.[2]
    , 'srcIP',  var_unpacked.[3]
| extend
var_decoded = base64_decodestring("S3VzdG8=")
```

#### New Style

```js
print bag_pack(
    "Level"
    , "Information"
    , "ProcessID"
    , 1234, "Data"
    , bag_pack("url", "www.bing.com")
)

| extend AdditionalFields = bag_pack(
    'Action',   var_action
    , 'type',   var_unpacked.[0]
    , 'proto',  var_unpacked.[1]
    , 'srcIF',  var_unpacked.[2]
    , 'srcIP',  var_unpacked.[3]
| extend
var_decoded = base64_decode_tostring("S3VzdG8=")
```

Both the function alias ```pack``` and ```base64_decodestring``` are deprecated among many other aliases. If not replaces timely these deprecated values can result in faulty code.
Especially when writing Analytic Rules for Microsoft Sentinel this can become painful when suddenly alerts are not triggered anymore.

### 10. Avoid using hidden characters

Although this doesn’t sound logical it actually is an important one to focus on. I have encountered several occurrence where it seemed that a normal _whitespace_ character was replaced with an empty value.

But what actually happened was that the whitespace being replaced was from another character set. After replacing this whitespace with a normal whitespace the query was broken.

This resulted in many hours of frustration and troubleshooting.

#### Code Example

```js
 | extend var_parsedString = replace(DeviceCustomString3, " ", "")
```

#### New Style

```js
\\ whitespace from another character set
| extend var_parsedString = replace_string(DeviceCustomString3, "\x20", "")
```

In the _code example_ the whitespace between the ```" "``` is actually a whitespace from another character set. By replacing the ```" "``` with the hex value for this whitespace ```"\x20"``` it becomes visible what value is being parsed in the ```DeviceCustomString3``` field.

## Bringing it all together

As a final example I have taken some code fragments from a recent ASim Parser of the Microsoft Sentinel GitHub repository and show the inconsistency in just _**one**_ of the many parsers.

### Example: ASimDnsGcp.yaml

```js
// https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry
let GCPSeverityTable=datatable(severity_s:string,EventSeverity:string)
["DEFAULT","Informational",
"DEBUG","Informational",
"INFO","Informational",
"NOTICE","Medium",
"WARNING","Medium",
"ERROR","High",
"CRITICAL","High",
"ALERT","High",
"EMERGENCY","High"
];
let DNSQuery_GcpDns=(disabled:bool=false){
    GCP_DNS_CL | where not(disabled)
    | project-away MG, ManagementGroupName, RawData, SourceSystem, Computer
    | where resource_type_s == "dns_query"
    | lookup GCPSeverityTable on severity_s
    | project-rename
        DnsQueryTypeName=payload_queryType_s,
        DnsResponseName=payload_rdata_s,.
        EventResultDetails=payload_responseCode_s,
        NetworkProtocol=payload_protocol_s,
        SrcIpAddr=payload_sourceIP_s,
        EventOriginalUid=insert_id_s
    | extend
        DnsQuery=trim_end(@'\.',payload_queryName_s),
        EventCount=int(1),
        EventProduct='Cloud DNS',
        EventVendor='GCP',
        EventSchema = 'Dns',
        Dvc="GCPDNS" ,
        EventType = iif (resource_type_s == "dns_query", "Query", resource_type_s),
        EventResult=iff(EventResultDetails=~'NOERROR','Success','Failure'),
        EventSubType='response',
        EventEndTime=todatetime(timestamp_t)
    | extend
        EventStartTime = EventEndTime,
        EventResult = iff (EventResultDetails=~'NOERROR','Success','Failure')
   // -- Aliases
    | extend.
        Domain=DnsQuery,
        IpAddr=SrcIpAddr,
        Src=SrcIpAddr
   // Backward Computability
    | project-away *_s, *_d, *_b, *_t
    };
```

### New Style

```js
https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry
let GCPSeverityTable = datatable(severity_s:string,EventSeverity:string) [
    "DEFAULT",     "Informational"
    , "DEBUG",     "Informational"
    , "INFO",      "Informational"
    , "NOTICE",    "Medium"
    , "WARNING",   "Medium"
    , "ERROR",     "High"
    , "CRITICAL",  "High"
    , "ALERT",     "High"
    , "EMERGENCY", "High"
];

let DNSQuery_GcpDns = (disabled:bool = false) {
  GCP_DNS_CL
  | where not(disabled)
  | project-away 
      MG
      , ManagementGroupName
      , RawData
      , SourceSystem
      , Computer
  | where resource_type_s == "dns_query"
  | lookup GCPSeverityTable on severity_s
  | project-rename
      DnsQueryTypeName     = payload_queryType_s
      , DnsResponseName    = payload_rdata_s
      , EventResultDetails = payload_responseCode_s
      , NetworkProtocol    = payload_protocol_s
      , SrcIpAddr          = payload_sourceIP_s
      , EventOriginalUid   = insert_id_s    
  | extend
      DnsQuery       = trim_end(@'\.', payload_queryName_s) 
      , EventCount   = int(1)
      , EventProduct = 'Cloud DNS'
      , EventVendor  = 'GCP'
      , EventSchema  = 'Dns'
      , Dvc          = "GCPDNS"
      , EventType    = iif(resource_type_s == "dns_query", "Query", resource_type_s)
      , EventResult  = iff(EventResultDetails =~ 'NOERROR', 'Success', 'Failure')
      , EventSubType = 'response'
      , EventEndTime = todatetime(timestamp_t)
  // Add recommended aliases
  | extend 
      Domain   = DnsQuery
      , IpAddr = SrcIpAddr
      , Src    = SrcIpAddr
  | project-away 
      *_s
      , *_d
      , *_b
      , *_t
};
```

#### Explanation

- [x] In the first ```let``` statement spaces has been added before and after the scalar operator ```=``` Inside the ```datatable``` the whitespaces have been added behind the comma and the opening bracket has moved to the same line as the statement. The closing bracket has been placed on a new line.

- [x] In the ```project-away``` and ```project-rename``` tabular operators the array of values is distributed to multiple lines. Each value in the array starts with a comma.

- [x] On the line where the ```EventType``` and ```EventResult``` fields are extended the spacing behind the scalar function ```iff``` is used inconsistently.

- [x] The last update according to this _style guide_ is using consistency in the comments, removing the ```--```
