# Drill User Agent Parser
Drill UDF for parsing User Agent Strings.
This function is based on Niels Basjes Java library for parsing user agent strings which is available here:  https://github.com/nielsbasjes/yauaa.

## Usage
Using this function is fairly simple. The function `parse_user_agent()` takes a user agent string as an argument and returns a map of the available fields.  Note that not every field will be present in every user agent string. 
```
SELECT parse_user_agent( columns[0] ) as ua 
FROM dfs.`/Users/cgivre/drill-httpd/ua.csv`;
```
The query above returns:
```
{
  "DeviceClass":"Desktop",
  "DeviceName":"Macintosh",
  "DeviceBrand":"Apple",
  "OperatingSystemClass":"Desktop",
  "OperatingSystemName":"Mac OS X",
  "OperatingSystemVersion":"10.10.1",
  "OperatingSystemNameVersion":"Mac OS X 10.10.1",
  "LayoutEngineClass":"Browser",
  "LayoutEngineName":"Blink",
  "LayoutEngineVersion":"39.0",
  "LayoutEngineVersionMajor":"39",
  "LayoutEngineNameVersion":"Blink 39.0",
  "LayoutEngineNameVersionMajor":"Blink 39",
  "AgentClass":"Browser",
  "AgentName":"Chrome",
  "AgentVersion":"39.0.2171.99",
  "AgentVersionMajor":"39",
  "AgentNameVersion":"Chrome 39.0.2171.99",
  "AgentNameVersionMajor":"Chrome 39",
  "DeviceCpu":"Intel"
}
```
The function returns a Drill map, so you can access any of the fields using Drill's table.map.key notation.  For example, the query below illustrates how to extract a field from this map and summarize it:

```
SELECT uadata.ua.AgentNameVersion AS Browser,
COUNT( * ) AS BrowserCount
FROM (
   SELECT parse_user_agent( columns[0] ) AS ua
   FROM dfs.drillworkshop.`user-agents.csv`
) AS uadata
GROUP BY uadata.ua.AgentNameVersion
ORDER BY BrowserCount DESC
```

### Installation and Dependencies
To install this function, first download the contents of this repository and build it using maven.
```
> git clone https://github.com/cgivre/drill-useragent-function.git
> cd drill-useragent-function
> mvn clean package -DskipTests
> cp ./target/*.jar <drill-path>/jars/3rdparty
```
Make sure you replace `<drill-path>` with your actual path to your drill installation.  

Next, you will have to download and build the UA parser.  Navigate out of the function folder and:
```
> git clone https://github.com/nielsbasjes/yauaa.git
> cd yauaa
> mvn clean package -DskipTests
> cp <path-to-yauaa>/analyzer/target/yauaa-0.11-SNAPSHOT.jar <drill-path>/jars/3rdparty
> cp <path-to-yauaa>/analyzer/target/yauaa-0.11-SNAPSHOT-udf.jar <drill-path>/jars/3rdparty
```
