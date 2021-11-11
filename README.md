# Oxylabs’ Residential Proxies integration with Selenium using Java

[<img src="https://img.shields.io/static/v1?label=&message=Java&color=brightgreen" />](https://github.com/topics/java) [<img src="https://img.shields.io/static/v1?label=&message=Selenium&color=orange" />](https://github.com/topics/selenium) [<img src="https://img.shields.io/static/v1?label=&message=Web-Scraping&color=yellow" />](https://github.com/topics/web-scraping) [<img src="https://img.shields.io/static/v1?label=&message=Rotating%20Proxies&color=blueviolet" />](https://github.com/topics/rotating-proxies)

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Proxy Authentication](#proxy-authentication)
- [Testing Proxy Connection](#testing-proxy-connection)
- [Understanding the Code](#understanding-the-code)

## Introduction

Integrating proxies that need authorization using the Selenium framework and Java programming 
language can be challenging. 

This tutorial contains complete code demonstrating how [Oxylabs’ Residential Proxies](https://oxylabs.io/products/residential-proxy-pool) can be 
easily integrated with Selenium using Java. 

## Requirements

To make this integration easier, we used [BrowserMob Proxy](https://github.com/lightbody/browsermob-proxy) 
as a middle layer. It runs proxies locally in JVM and allows chaining of Oxylabs' authenticated proxies. 
If you’re using Maven, add this dependency to the `pom.xml` file:

```xml

<dependency>
    <groupId>net.lightbody.bmp</groupId>
    <artifactId>browsermob-core</artifactId>
    <version>2.1.5</version>
</dependency>
```

The other library used in this project – [WebDriverManager](https://github.com/bonigarcia/webdrivermanager), 
is optional. It just makes downloading and setting up [Chrome Driver](https://chromedriver.chromium.org/downloads) 
easier. To use this library, include the following dependency in `pom.xml` file:

```xml

<dependency>
    <groupId>io.github.bonigarcia</groupId>
    <artifactId>webdrivermanager</artifactId>
    <version>5.0.2</version>
</dependency>
```

If you don’t want to use WebDriverManager, download the Chrome Driver and set the system property as follows:

```java
System.setProperty("webdriver.chrome.driver","/path/to/chromedriver");
```

## Proxy Authentication

Open [ProxySetup.java](src/main/java/ProxySetup.java) file and update your username, password, and endpoint.

```java
static final String ENDPOINT="pr.oxylabs.io:7777";
static final String USERNAME="yourUsername";
static final String PASSWORD="yourPassword";
```

You shouldn’t include the prefix `customer-` in the `USERNAME.` This will be added in the code for country-specific proxies.

## Testing Proxy Connection

Open this project in IDE, open the [ProxySetup.java](src/main/java/ProxySetup.java) file, and run the `main()` function. 
This will print two IP addresses.

- The first IP address will be completely random;
- The second IP address will be a country-specific IP address in Germany.

## Getting Country Specific Proxy

Open [ProxyDemo.java](src/main/java/ProxyDemo.java) file and send a two-letter country code to the function `CountrySpecficIPDemo`.

```java
countrySpecificIPDemo("DE");
```

The value of this parameter is a case-insensitive country code in two-letter [3166-1 alpha-2 format](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2). For example, `DE` for 
Germany, `GB` for the United Kingdom, etc. For more details, see Oxylabs’ [documentation](https://developers.oxylabs.io/residential-proxies/?java#select-country).

## Understanding the Code

All the complexity of setting up BrowserMob Proxy and Chrome Options is hidden in
the [ProxyHelper](src/main/java/ProxyHelper.java) class.

In most cases, you should be able to use this file directly without any change.

To create a Chrome Driver instance, go through a two-step process as follows:

First, create an instance of `BrowserMobProxyServer`. This is where you need to provide the proxy endpoint, username, and password.

The fourth parameter is a two-letter country code. If you don’t need a country-specific proxy, set it to `null`:

```java
BrowserMobProxyServer proxy=ProxyHelper.getProxy(
        ProxySetup.ENDPOINT,
        ProxySetup.USERNAME,
        ProxySetup.PASSWORD,
        countryCode)
```

Next, call the `ProxyHelper.getDriver()` function.

This function takes up two parameters -`BrowserMobProxyServer` and a `boolean` headless. To run the browser in headless mode, send `true`:

```java
WebDriver driver=ProxyHelper.getDriver(proxy,true);
```

`driver` is an instance of Chrome Driver.

Now, you should write your code to use the Chrome Driver.

Before exiting, remember to close the driver and stop the proxy:

```java
driver.quit();
proxy.stop(); 
```

If you're having any trouble integrating Oxylabs’ Residential Proxies with Selenium and this guide didn't help you – feel free to contact our customer support at support@oxylabs.io.

