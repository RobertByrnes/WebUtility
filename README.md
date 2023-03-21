<h1 align="center">WebUtility</h1>

<p align="center">

<img src="https://img.shields.io/badge/made%20by-RobertByrnes-blue.svg" >

<img src="https://img.shields.io/badge/stability-wip-red.svg" >

<!-- <img src="https://img.shields.io/npm/v/vue2-baremetrics-calendar">

<img src="https://img.shields.io/badge/vue-2.6.10-green.svg"> -->

<!-- <img src="https://badges.frapsoft.com/os/v1/open-source.svg?v=103" >

<img src="https://img.shields.io/github/stars/silent-lad/Vue2BaremetricsCalendar.svg?style=flat">

<img src="https://img.shields.io/github/languages/top/silent-lad/Vue2BaremetricsCalendar.svg">

<img src="https://img.shields.io/github/issues/silent-lad/Vue2BaremetricsCalendar.svg"> -->

<img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat">
</p>

![World Wide Web](www.jpg?raw=true "WWW")

_Web Utilities to aid iot development **[Docs](https://emulation.com)**_

# WebUtility  

### Some very handy classes for secure IoT development - mainly around making requests ...

<br>

# HTTPS

This class starts by defining constants and functions related to the HTTPS protocol. The constants define various response codes, headers, and methods used in an HTTPS request. The functions allow for making a GET or POST request with a TinyGSM modem and ArduinoHttpClient. 
## Functions

- The ```get()``` function takes a ModemDriver and HttpClientDriver as parameters and makes a GET request to the specified resource. It throws an error if the response status code is not 200. 

- The ```postJSON()``` function takes a ModemDriver, HttpClientDriver, endpoint, request body, and optional current token as parameters and makes a POST request with the given JSON body. It also reads the response headers and checks if the response is chunked. It throws an error if the response status code is not 200.

- The ```print()``` function, takes in a ModemDriver object, HttpClientDriver object, Headers object, and a string requestBody as parameters. It checks if the GPRS connection is active, then sets the http response timeout, keeps the connection alive, begins the request, posts to the endpoint, sends the headers, ends the request, and writes the request body. It then checks the status code of the response and logs it. If the response is OK, it reads the response and returns it. Otherwise, it throws an error. 

- The ```download()``` function takes in a ModemDriver object, HttpClientDriver object, SPIFFSType object, resource, filePath, and knownCRC32 as parameters. It checks if the GPRS connection is active, then attempts to download the file. It checks the status code of the response and logs it. If the response is OK, it removes any old files with the same name, gets the content length, opens a file, reads the bytes from the response, calculates the CRC32, closes the file, logs the information, and waits 3 seconds. It then checks if the content length matches the read length and returns true or false accordingly. Otherwise, it throws an error. 

- ```readHeaders()``` takes in an HttpDriver object as a parameter. It reads the HTTP response headers and logs them. If there is an error, it throws an error. 

- ```responseOK()``` takes in a status code as a parameter. It checks if the status code is in the accepted group of codes and returns true or false accordingly. 

- ```sendHeaders()```, takes in an HttpDriver object and Headers object as parameters. It sends the headers.

## Tests
This code is setting up a test to check if the get method returns a non-empty string on success.
It begins by defining the constants and variables that will be used in the test, such as the APN, GPRS user and password, 
host name, port, LED pin, fake resource, modem driver mock, GSM transport layer mock, secure layer mock, HTTP client mock, 
modem class, HTTPS class, SPIFFS class, and method logger. It then defines a success response string and a request body string. 

- ```setUp()``` function resets the modem driver mock and HTTP client mock. 
- ```tearDown()``` function is defined, which does not contain any code. 
- ```testGetMethodReturnsNonEmptyStringOnSuccess()``` function is defined, which tests if the get method returns a non-empty string on success. This function first checks if the GPRS connection is 
established, then calls the get method with the modem driver mock, HTTP client mock, and fake resource. It then checks if 
the response status code is 200 and if the response body is equal to the success response string. If both of these conditions 
are true, it asserts that the response is equal to the success response string.
- ```testGetMethodThrowsOnNone200Response()``` and ```testGetMethodThrowsIfModemNotConnected()```, are testing the get method of the https class. The first function tests that an exception is thrown when the response status code is not 200 (in this case 403). The second function tests that an exception is thrown if the modem is not connected. 
- ```testPostJSONMethodReturnsNonEmptyStringOnSuccess()``` and ```testPostJsonMethodReturnsEmptyStringIfResponseBodyLengthZero()```, are testing the postJSON method of the https class. The first function tests that a non-empty string is returned on success. The second function tests that an empty string is returned if the response body length is zero. 
- ```testPostJsonMethodThrowsOnNone200Response()``` and ```testPrintMethodThrowsOnNone200Response()```, are testing the print method of the https class. The first function tests that an exception is thrown when the response status code is not 200. The second function tests that a non-empty string is returned on success.
- ```testPrintMethodThrowsIfModemNotConnected()```, tests that an exception is thrown if the modem is not connected. The next two tests, ```testResponseOKMethodReturnsTrueIfStatusCodeInSwitch()``` and ```testResponseOKMethodReturnsFalseIfStatusCodeNotInSwitch()```, test the responseOK method which checks if the status code is in a switch statement. The following two tests, ```testReadHeadersDoesNotThrowIfHeadersValid()``` and ```testReadHeadersThrowsIfHeadersInvalid()```, test the readHeaders method which reads the headers from the http client. The last four tests, ```testDownloadMethodReturnsTrueIfDownloadSucceedsWithNoContentLengthHeader()```, ```testDownloadMethodReturnsTrueIfDownloadSucceedsWithContentLengthHeader()```, ```testDownloadMethodReturnsFalseIfContentLengthCheckNotSatisfied()```, and ```testDownloadMethodThrowsOnNone200Response()```, test the download method which downloads a resource from the http client. Finally, ```testDownloadMethodThrowsIfModemNotConnected()``` tests that an exception is thrown if the modem is not connected.
---

<br>

# Modem
This code is defining pins and setting up a modem driver for an ESP32 LilyGo-T-Call-SIM800 SIM800L_IP5306_VERSION_20190610 (v1.3) board. The code defines the pins used to control the modem, such as ```MODEM_RST, MODEM_PWRKEY, MODEM_POWER_ON, MODEM_UART_BAUD, MODEM_TX, MODEM_RX, I2C_SDA, I2C_SCL, IP5306_ADDR, IP5306_REG_SYS_CTL0, TINY_GSM_RX_BUFFER, and MODEM_LED_PIN```. It also includes definitions for a SIM808 modem, but these are commented out. 

The code then includes Wire.h and TinyGsmClient.h libraries, and defines a class called Modem. This class contains functions for setting up the modem, connecting to the GPRS network, connecting to the APN, logging connection information, and logging modem information. It also contains a setupPMU function which is used to configure the power settings for the board.

## Functions
This code is a template for setting up a modem.
- The ```setupModem()``` function sets up the pins for the modem, such as the reset pin and power key pin.
- The ```connect()``` function attempts to connect to the GPRS network using the given APN, user name, and password. It also checks for network availability and verifies that the connection was successful.
- The ```awaitNetworkAvailability()``` and ```verifyConnected()``` functions are used to check for network availability and verify the connection respectively.
- The ```logConnectionInformation()``` and ```logModemInformation()``` functions are used to log information about the connection and the modem.
- Finally, the ```setupPMU()``` function is used to configure the power settings for the SIM800L_IP5306_VERSION_20190610 (v1.3) board. 

## Tests
This code is testing the functionality of a modem class. The ```setUp()``` and ```tearDown()``` functions are used to reset the modemDriverMock object before each test. The following tests check if the modemClass can successfully connect to a network, connect to an APN, verify that it is connected, log connection information, and log modem information without throwing any errors. The last two tests check if the ```setupPMU()``` function returns true or false depending on the return value of the Wire object. Finally, the ```testConnectReturnsTrueIfModemConnectedToAPN()``` test checks if the modemClass can successfully connect to an APN and returns true if it does.

---

<br>

# Semver
This code is a class called Semver which contains a static method called ```versionCompare()```. This method takes two strings, v1 and v2, as parameters and compares them to determine which one is larger. It does this by looping through each string and storing the numeric parts of the versions in variables vnum1 and vnum2. It then compares these values and returns 1 if v2 is smaller, -1 if v1 is smaller, or 0 if they are equal. The class also has private constructors and destructors which prevent it from being instantiated, it is therefore called statically for ease.
