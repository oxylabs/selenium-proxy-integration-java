# Oxylabs’ Residential Proxies integration with Selenium using Java

[<img src="https://img.shields.io/static/v1?label=&message=Java&color=brightgreen" />](https://github.com/topics/java) [<img src="https://img.shields.io/static/v1?label=&message=Selenium&color=orange" />](https://github.com/topics/selenium) [<img src="https://img.shields.io/static/v1?label=&message=Web-Scraping&color=yellow" />](https://github.com/topics/web-scraping) [<img src="https://img.shields.io/static/v1?label=&message=Rotating%20Proxies&color=blueviolet" />](https://github.com/topics/rotating-proxies)

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Proxy Authentication](#proxy-authentication)
- [Testing Proxy Connection](#testing-proxy-connection)
- [Understanding the Code](#understanding-the-code)

## Introduction

Integrating proxies that need authorization with Selenium can be little tricky with Java.

This tutorial contains complete code that demonstrates how Oxylabs’ Residential Proxies can be
integrated with Selenium using Java.

## Requirements

To make this easier, this code
uses [BrowserMob Proxy](https://github.com/lightbody/browsermob-proxy) as a middle layer. This
proxy runs locally in JVM and allows chaining of Oxyalabs' authenticated proxies.

If you are using Maven, add this dependency to the `pom.xml` file:

```xml

<dependency>
    <groupId>net.lightbody.bmp</groupId>
    <artifactId>browsermob-core</artifactId>
    <version>2.1.5</version>
</dependency>
```

The other library used in this project is optional. This library
is [WebDriverManager](https://github.com/bonigarcia/webdrivermanager).

It makes downloading and setting up [Chrome Driver](https://chromedriver.chromium.org/downloads)
much easier.

To use this library, include the following dependency in `pom.xml` file:

```xml

<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.0.2</version>
</dependency>
```

If you do not wish to use WebDriverManager, download the Chrome Driver and set the system property
as follows:

```java
System.setProperty("webdriver.chrome.driver","/path/to/chromedriver");
```

## Proxy Authentication

Open [ProxySetup.java](src/main/java/ProxySetup.java) file and update your username, password and
endpoint.

```java
static final String ENDPOINT="pr.oxylabs.io:7777";
static final String USERNAME="yourUsername";
static final String PASSWORD="yourPassword";
```

Do not include the prefix `customer-` in the `USERNAME`. This will be added in the code for country
specific proxies.

## Testing Proxy Connection

Open this project in IDE, open ProxyDemo.java file run the `main()` function.

This will print two IP addresses.

- The first IP address will be completely random.
- The second IP address will be a country specific IP in Germany.

## Getting Country Specific Proxy

Open [ProxyDemo.java](src/main/java/ProxyDemo.java) file.

Send the two letter country code to the function `CountrySpecficIPDemo`.

```java
countrySpecificIPDemo("DE");
```

The value of this parameter is a case-insensitive country code in 2-letter [3166-1 alpha-2 format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). 

For example, `DE` for Germany, `GB` for the United Kingdom, etc.

For more details, see Oxylabs [documentation](https://developers.oxylabs.io/residential-proxies/?java#select-country).

## Understanding the Code

All the complexity of setting up BrowserMob Proxy and Chrome Options have been hidden in
the [ProxyHelper](src/main/java/ProxyHelper.java) class.

In most of the cases, you should be able to use this file directly without any change.

To create Chrome Driver instance, go through a two-step process as following.

First, Create an instance of `BrowserMobProxyServer`. This is where you need to provide the proxy
endpoint, username, and password.

The fourth parameter is two letter country code. If you do not need country specific proxy, set it
to `null`:

```java
BrowserMobProxyServer proxy=ProxyHelper.getProxy(
        ProxySetup.ENDPOINT,
        ProxySetup.USERNAME,
        ProxySetup.PASSWORD,
        countryCode)
```

Next, call the `ProxyHelper.getDriver()` function.

This function takes up two parameters--`BrowserMobProxyServer` and a `boolean` headless.  To run 
the browser in headless mode, send `true`:

```java
WebDriver driver=ProxyHelper.getDriver(proxy,true);
```

`driver` is an instance of Chrome Driver.

Write your code to use the Chrome Driver now.

Before exiting, remember to close the driver and stop the proxy:

```java
driver.quit();
proxy.stop(); 
```

If you're having any trouble integrating proxies with Selenium and this guide didn't help you -
feel free to contact Oxylabs customer support at support@oxylabs.io.