[comment]: # "Auto-generated SOAR connector documentation"
# GreyNoise

Publisher: GreyNoise  
Connector Version: 2\.1\.2  
Product Vendor: GreyNoise  
Product Name: GreyNoise  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 4\.9\.39220  

This app provides investigative capabilities using the GreyNoise plugin

[comment]: # " File: readme.md"
[comment]: # ""
[comment]: # "  Licensed under Apache 2.0 (https://www.apache.org/licenses/LICENSE-2.0.txt)"
[comment]: # ""
## Playbook Backward Compatibility

-   Version 2.0.0 of this application is a complete rewrite and is not backward compatible with
    version 1.0.0. Hence, it is requested to the end-user to please update their existing playbooks
    by re-inserting \| modifying \| deleting the corresponding action blocks to ensure the correct
    functioning of the playbooks created on the earlier versions of the app. If the end-user does
    not want to upgrade their playbooks, they can remain on or downgrade to the old version(v1.0.0).

## Description

The GreyNoise Enrichment plugin for Phantom enriches observables to identify activity associated
with mass-internet scanning, creating more time to investigate other higher priority observables.
This enrichment provides context into IP behavior: intent, tags, first seen, last seen, geo-data,
ports, OS, and JA3.  
  
The GreyNoise Enrichment plugin for Phantom requires an API key. Set up an account to receive an API
key and find GreyNoise documentation here: <https://developer.greynoise.io/>

## Actions

#### lookup ip

Check to see if a given IP has been seen by GreyNoise engaging in internet scanning behavior.

#### riot lookup ip

Identifies IPs from known benign services and organizations that commonly cause false positives.

#### community lookup ip

An action requiring at least a free community API key to query IPs in the GreyNoise dataset and
retrieve a subset of the IP reputation data returned by the lookup ip and lookup reputation actions.
A free API key can be obtained at <https://www.greynoise.io/viz/signup>

#### lookup ips

Check whether IP addresses in a set have been seen engaging in internet scanning behavior. This
action is similar to *lookup ip* except that it processes more than one IP at a time. IPs should be
comma-separated.

#### ip reputation

Delivers full IP context: time ranges, IP metadata (network owner, ASN, reverse DNS pointer,
country), associated actors, activity tags, and raw port scan and web request information.

#### gnql query

GreyNoise Query Language (GNQL) uses Lucene deep under the hood. GNQL enables users to make complex
and one-off queries against the GreyNoise dataset.  
For more information, please visit: <https://developer.greynoise.io/reference#gnqlquery-1>

#### on poll

Retrieves GNQL query results on a set interval. The default number of results returned is 25.  
Notes:

-   The on poll action will spawn a container for each result returned. Phantom performance may be
    degraded if an overly large query is used.

-   Potentially useful queries may include ones that limit results to assets owned by your
    organization, such as:

-   -   metadata.organization:your_organization classification:malicious
    -   8.8.8.0/30 (replace with your address block) classification:malicious

-   To test your query or to learn more about GNQL queries, please visit
    <https://developer.greynoise.io/reference#gnqlquery-1>

#### test connectivity

Test connectivity to GreyNoise. Requires a valid paid or free community API key.

## Legal

For terms and legal information, please visit <https://greynoise.io/terms>


### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a GreyNoise asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**api\_key** |  required  | password | API Key for GreyNoise
**on\_poll\_query** |  optional  | string | GNQL query to use for the on poll action
**on\_poll\_size** |  optional  | numeric | The number of results to return for the on poll action

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using the supplied configuration  
[lookup ip](#action-lookup-ip) - Lookup IP using GreyNoise API Quick Check Endpoint  
[riot lookup ip](#action-riot-lookup-ip) - Lookup IP using GreyNoise's RIOT \(Rule It OuT\) endpoint  
[ip reputation](#action-ip-reputation) - Get full GreyNoise reputation and context for a specific IP  
[gnql query](#action-gnql-query) - Use the GreyNoise Query Language to run a query  
[lookup ips](#action-lookup-ips) - Lookup IPs using GreyNoise API Multi Quick Check Endpoint \(comma\-separated, limit 500 per request\)  
[on poll](#action-on-poll) - Get details on a specific GNQL query  
[community lookup ip](#action-community-lookup-ip) - Lookup IP using GreyNoise's free community endpoint  

## action: 'test connectivity'
Validate the asset configuration for connectivity using the supplied configuration

Type: **test**  
Read only: **True**

Tests the connection to the paid GreyNoise API\.

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'lookup ip'
Lookup IP using GreyNoise API Quick Check Endpoint

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**ip** |  required  | IP to query | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.ip | string |  `ip` 
action\_result\.data\.\*\.code | string | 
action\_result\.data\.\*\.code\_meaning | string | 
action\_result\.data\.\*\.noise | boolean | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'riot lookup ip'
Lookup IP using GreyNoise's RIOT \(Rule It OuT\) endpoint

Type: **investigate**  
Read only: **True**

RIOT identifies IPs from known benign services and organizations that commonly cause false positives in network security and threat intelligence products\. The collection of IPs in RIOT is continually curated and verified to provide accurate results\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**ip** |  required  | IP to query | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.ip | string |  `ip` 
action\_result\.data\.\*\.category | string | 
action\_result\.data\.\*\.name | string | 
action\_result\.data\.\*\.description | string | 
action\_result\.data\.\*\.explanation | string | 
action\_result\.data\.\*\.last\_updated | string | 
action\_result\.data\.\*\.logo\_url | string | 
action\_result\.data\.\*\.reference | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'ip reputation'
Get full GreyNoise reputation and context for a specific IP

Type: **investigate**  
Read only: **True**

Get more information about a given IP address\. Returns time ranges, IP metadata \(network owner, ASN, reverse DNS pointer, country\), associated actors, activity tags, raw port scan, and web request information\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**ip** |  required  | IP to query | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.ip | string |  `ip` 
action\_result\.data\.\*\.seen | boolean | 
action\_result\.data\.\*\.classification | string | 
action\_result\.data\.\*\.first\_seen | string | 
action\_result\.data\.\*\.last\_seen | string | 
action\_result\.data\.\*\.actor | string | 
action\_result\.data\.\*\.tags\.\* | string | 
action\_result\.data\.\*\.metadata\.\* | string | 
action\_result\.data\.\*\.raw\_data\.\* | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'gnql query'
Use the GreyNoise Query Language to run a query

Type: **investigate**  
Read only: **True**

Please refer to <a target="\_blank" rel="noopener noreferrer" href="https\://developer\.greynoise\.io/reference\#gnqlquery\-1">https\://developer\.greynoise\.io/reference\#gnqlquery\-1</a> for further information\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**query** |  required  | GNQL query | string |  `greynoise query` 
**size** |  required  | The number of results to return \(warning\: returning over 1000 results may degrade widget performance\) | numeric | 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.query | string |  `greynoise query` 
action\_result\.parameter\.size | numeric | 
action\_result\.data\.\*\.ip | string |  `ip` 
action\_result\.data\.\*\.seen | boolean | 
action\_result\.data\.\*\.classification | string | 
action\_result\.data\.\*\.first\_seen | string | 
action\_result\.data\.\*\.last\_seen | string | 
action\_result\.data\.\*\.actor | string | 
action\_result\.data\.\*\.tags\.\* | string | 
action\_result\.data\.\*\.metadata\.\* | string | 
action\_result\.data\.\*\.raw\_data\.\* | string | 
action\_result\.complete | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'lookup ips'
Lookup IPs using GreyNoise API Multi Quick Check Endpoint \(comma\-separated, limit 500 per request\)

Type: **investigate**  
Read only: **True**

Returns quick check information for multiple IPs\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**ips** |  required  | IPs to query, comma\-separated list allowed | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.ips | string |  `ip` 
action\_result\.data\.\*\.code | string | 
action\_result\.data\.\*\.code\_meaning | string | 
action\_result\.data\.\*\.noise | boolean | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'on poll'
Get details on a specific GNQL query

Type: **ingest**  
Read only: **True**

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'community lookup ip'
Lookup IP using GreyNoise's free community endpoint

Type: **investigate**  
Read only: **True**

The Community API provides community users with a free tool to query IPs in the GreyNoise dataset and retrieve a subset of the full IP context data returned by the IP Lookup API\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**ip** |  required  | IP to query | string |  `ip` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.parameter\.ip | string |  `ip` 
action\_result\.data\.\*\.classification | string | 
action\_result\.data\.\*\.name | string | 
action\_result\.data\.\*\.link | string | 
action\_result\.data\.\*\.last\_seen | string | 
action\_result\.data\.\*\.message | string | 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.summary | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 