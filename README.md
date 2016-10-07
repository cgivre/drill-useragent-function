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
> cp <path-to-yauaa>/analyzer/target/yayaa-0.9-SNAPSHOT.jar <drill-path>/jars/3rdparty
> cp <path-to-yauaa>/devtools/target/devtools-0.9-SNAPSHOT.jar <drill-path>/jars/3rdparty
```
Finally, In order for this function to work, in addition to the UA parser, you will need to add the following JARs to `<drill path>/jars/3rdparty`.

* ANTlr Version 4 (http://www.antlr.org/download/antlr-4.5.3-complete.jar)
* Commons Collections version 4 (http://www.gtlib.gatech.edu/pub/apache//commons/collections/binaries/commons-collections4-4.1-bin.tar.gz)
* Springframework (http://www.java2s.com/Code/JarDownload/spring/spring-2.0-core.jar.zip)
* Snakeyaml (http://central.maven.org/maven2/org/yaml/snakeyaml/1.5/snakeyaml-1.5.jar)

Simply download and copy all those .jar files into your `<drill-path>/jars/3rdparty` directory.
