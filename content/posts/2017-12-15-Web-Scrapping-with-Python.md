---
weight: 15
title: "Web Scrapping with Python"
date: 2017-12-15 13:00:00 +0530
description: ""
type: post
showTableOfContents: true
---

Internet has superabundant information and knowledge but to extract that knowledge in organised, scalable and systemetic way for data analysis and other research purpose is known as [Web Scrapping](https://en.wikipedia.org/wiki/Web_scraping).

Web Scrapping automatically extracts the data from a particular website and present it in user desired form. We can scrap many things from the web like Stock updates, Price of any product from Amazon. In this post I'll scrap news updates from my [college's website](http://www.iitbbs.ac.in).

## Getting Started

We can use almost any scripting language for web scrapping however the popular options are Javascript and Python. Both Javascript and Python have rich set of scrapping libraries like [CheerioJS](https://github.com/cheeriojs/cheerio) for Javascript and [Scrapy](https://scrapy.org), [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) for Python.

I'll use Python and BeautifulSoup for this post because both Python and BeautifulSoup are beginner friendly.

- For Mac and Linux users, Python is preinstalled.
- Windows users can install Python from [official website](https://python.org).

For installing BeautifulSoup library, simply enter following command in terminal.

```bash
pip install beautifulsoup4
```

## Finding Target from HTML

All the news & updates of IIT Bhubaneswar are enlisted on [this](http://www.iitbbs.ac.in/news.php) webpage. Visit this Webpage and press `Ctr + Shift + I` to inspect source code of the webpage. Now click on the the mouse button present on the top-left part of inspector panel and hover over list of news updates. We can see in inspector panel that all news and updates of that webpage are inside an unordered list with class _rectlist_.

```html
<ol class="rectlist"></ol>
```

Thus we have to scrap all list items present in aforementioned unordered list.

## Writing script for scrapping

```python
# imporing library for fetching HTML content of webpage
import urllib2
# For scrapping list items txt and links from raw HTML
from bs4 import BeautifulSoup

# Target URL
url = 'http://www.iitbbs.ac.in/news.php'

# Fetching HTML content from url and storing it in 'page' variable
page = urllib2.urlopen(url)

# Parsing HTML with beautifulsoup and storing it in 'soup' variable
soup = BeautifulSoup(page, 'html.parser')

# Finding Unordered list with rectlist class and storing it in newsOl variable
newsOl = soup.find('ol', attrs={'class': 'rectlist'})

# Finding all list items of unordered list and storing them in newsLi variable
newsLi = newsOl.find_all('li')

# Printing text and href address of all list items
for link in newsLi:
	# Storing text of link in news variable
	news = link.text.strip()

	# Storing href address in link variable.
	# Some href are relatively stored so we have to screen them and append absolute path in href address.
	if link.find('a').get('href')[:4] != 'http':
		link = 'http://www.iitbbs.ac.in/' + link.find('a').get('href')
	else:
		link = link.find('a').get('href')

	# Printing news and link to terminal/shell.
	print(news + '\n' + link + '\n')
```

After running this script we'll get required result in terminal.

You can learn more about BeautifulSoup library by visiting it's [official documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/).

Other Resources:

- [Geeksforgeeks](http://www.geeksforgeeks.org/implementing-web-scraping-python-beautiful-soup/)
- [Analytics Vidhya](https://www.analyticsvidhya.com/blog/2015/10/beginner-guide-web-scraping-beautiful-soup-python/)

**Update:** I recently came to know another very good resource(<https://likegeeks.com/python-web-scraping>) for Web Scrapping. I think you may also like it.

**Warning:** Scrapping data from certain website may violate their Terms and Conditions or Privacy Policy. Please check such things before deploying any scrapper. Most of the websites also have a file named _robots.txt_ on their server which lists all such webpages that shouldn't be crawled so check for such files before scrapping from any site. For e.g. robot.txt file of Google is present on this [link](https://www.google.co.in/robots.txt).

---

You can comment below any doubt / criticism or compliment. Also if you like this post please share it among your hacker friends.
Don’t forget to bookmark my blog and keep visiting frequently. Bonne journee!
