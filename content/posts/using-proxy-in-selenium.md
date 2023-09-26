+++
author = "fahamjv"
title = "Unlock Seamless Web Scraping with Selenium Wire Proxies"
date = "2022-12-17"
description = "Discover how to overcome IP bans and ensure smooth web scraping using Selenium Wire proxies. Learn to integrate Webshare proxies seamlessly and unlock a reliable solution for your web data extraction needs."
aliases = ["selenium-wire", "selenium-wire-proxies", "web-scraping", "proxy-integration", "webshare-proxies"]
+++

In my web scraping journey with [Selenium](https://pypi.org/project/selenium/), I encountered a roadblock when the website I was scraping suddenly returned a "403 Forbidden Access" error. It turned out that my VPS IP had been banned.

{{< figure src="/images/403-forbidden-error.png" alt="403 forbidden" align="center">}}

To overcome this issue and ensure seamless web scraping, I explored using proxy servers with Selenium. After some trial and error, I discovered a reliable solution: [Selenium Wire](https://pypi.org/project/selenium-wire/).

Selenium Wire is an extension of Selenium that offers additional features, including seamless proxy integration.

For proxy, i had prior experience with [Webshare](https://www.webshare.io/?referral_code=wbj58rdo8cqi) proxies; they were stable and decent. Plus, they offered a permanent free plan with 10 proxies! So, I wrote a simple API call to retrieve all the proxies from Webshare like this:

```python
import requests

def get_proxies(api_key, page=1, page_size=25):
    url = f"https://proxy.webshare.io/api/v2/proxy/list/?mode=direct&page={page}&page_size={page_size}"
    headers = {"Authorization": api_key}
    response = requests.get(url, headers=headers)

    if response.status_code == 200:
        proxies = []
        for proxy in response.json().get('results', []):
            proxies.append(f"http://{proxy['username']}:{proxy['password']}@{proxy['proxy_address']}:{proxy['port']}")
        return proxies

    print(f"Error fetching proxies: {response.status_code}")
    return []
```

To use these proxies with Selenium, follow these steps:

1. Fetch a random proxy from the list obtained using the `get_proxies` function.
2. Configure the Selenium WebDriver to use this proxy with Selenium Wire.

Here's a snippet of the code:

```python
from seleniumwire import webdriver
import random

def configure_selenium_with_proxy(proxy_list, chrome_options):
    proxy = random.choice(proxy_list)
    return webdriver.Chrome(options=chrome_options, seleniumwire_options={'proxy': proxy})
```

By implementing this approach with Selenium Wire, you can easily bypass IP bans and ensure uninterrupted web scraping.

