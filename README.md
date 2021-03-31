# StAX Testing

Project tests interlok-stax features

## What it does

This project is very simple and contains only one single channel with one workflow.

- The workflow has a polling trigger that produces a message every 10 seconds.
- An embedded scripting service creates a json file with user information with a random seed number for the username, first name and last name.
- Then a "STaX streaming service" convert the json payload to an XML payload and a "STaX get root element service" put the XML root into a metadata.
- A "STaX start document" is used to create a user-wrapper node, the user XML content is added to it, a random address XML, built in the same way as the user, is also added to it and a "STaX end document service" is used to finish the transaction.
- Finally an "Xpath service" extract the username and address number from the xml and a "If service" check if these values equals the values stored in metadata when the user and address were created, an exception is thrown if the validation failss.

![STaX Diagram](/interlok-stax-diagram.png "STaX Diagram")

The Embedded Scripting Service creates the json file message and add the randomly generated username and address number into metadata.
The random number is between 1 and 10. But "00" is added as a metadata when the its value is 9 to simulate some sporadic failures.

## Example

The generated user json file is like:

```
{"user":{  "username":"username-01",  "firstName":"firstName-01",  "lastName":"lastName-01"}}
```

And converted to an XML like:

```xml
<?xml version='1.0' encoding='UTF-8'?><user><username>username-01</username><firstName>firstName-01</firstName><lastName>lastName-01</lastName></user>
```

The generated address json file is like:

```
{"address":{  "number":"06",  "street":"Avenue 06",  "postcode":"000-06"}}
```

And converted to an XML like:

```xml
<?xml version='1.0' encoding='UTF-8'?><address><number>06</number><street>Avenue 06</street><postcode>000-06</postcode></address>
```

## Getting started

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`


The config is using a variables.properties to configure the tmp dir location.

```
tmpDir=./messages/tmp
```
