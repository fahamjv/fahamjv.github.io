+++
author = "fahamjv"
title = "Using proxy in Selenium"
date = "2022-12-17"
description = "How to use proxy in selenium-wire"
aliases = ["selenium-wire", "proxy", "crawling"]
+++

I'm crawling a website using [Selenium](https://pypi.org/project/selenium/) every 5 minutes to extract some data. After a few days, I noticed that the data is not updating anymore. So I curled the website and it returned 403 Forbidden Access!
![403 forbidden](https://fahamjv.com/images/403-forbidden-error.png)
yay, my VPS IP got banned. I tried to use some HTTP and HTTPS proxy servers in Selenium but that was so buggy! I couldn't make it after playing for hours with it!
After searching for a while, I found this thing called [selenium-wire](https://pypi.org/project/selenium-wire/). It's Selenium but with extra features!
I've used [Webshare](https://www.webshare.io/?referral_code=wbj58rdo8cqi) proxies before, it's stable and ok, plus they offer a permanent free plan with 10 proxies!
So, I wrote a simple API call to get all the proxies from webshare like this:

```python
def get_proxies():
    response = requests.get(
        "https://proxy.webshare.io/api/v2/proxy/list/?mode=direct&page=1&page_size=25",
        headers={"Authorization": PROXY_API_KEY}
    )
    proxies = []
    for proxy in response.json()['results']:
        url = "http://{username}:{password}@{proxy_address}:{port}".format(
            username=proxy['username'],
            password=proxy['password'],
            proxy_address=proxy['proxy_address'],
            port=proxy['port'],
        )
        proxies.append({'http' : url})

    return proxies
```

Then we need to choose one of the proxies randomly and assign it to the webdriver, using the seleniumwire_options argument.

```python
seleniumwire_options = {
    'proxy': {},
}
seleniumwire_options['proxy'] = random.choice(get_proxies())
driver = webdriver.Chrome(options=options,
    seleniumwire_options=seleniumwire_options)
```
Sim Sala Bim !!! We are not banned anymore

