# CS 4675 Web Crawler

## Overview
For HW1, I decided to choose Option 1.2 and write a web crawler of my own. This web crawler is written in Go and uses MongoDB as a web archive.

![[images/cmd-ui.png]]
## How it works
Here's the the data flow of the web crawler:
![[images/web-crawler-diagram.png]]

## Web Archive
To store the content of crawled web pages, I used MongoDB, a NoSQL database with a built-in Search Index functionality. 

I decided to store the `url`, `title`, and first 500 characters after the `<body>` tag as `content`. Then, I created a Search Index on the `title` and `content` fields, using MongoDB's standard keyword analyzer. This created an inverted index table which mapped keywords to webpages. 

The Search Tester GUI allowed me to query the web archive.
![[images/web-archive.png]]

## Performance
In my program, I used Go's built-in Time library to record the Crawl Statistics every minute. The statistics included:
- Crawl Speed: Pages / second
- Crawled to Queued Ratio / second
![images/crawl-speed-graph.png](file:///Users/alexafazio/Desktop/code-projects/gatech/cs-4675/HW1/images/crawl-speed-graph.png)

![images/crawl-ratio-graph.png](file:///Users/alexafazio/Desktop/code-projects/gatech/cs-4675/HW1/images/crawl-ratio-graph.png)
## Experience

### Pros
- Concurrently fetches and parses web pages
- Database inserts are concurrent
- Avoids loops and dead ends
- Ignores script tags
- Gracefully handles invalid urls, page not founds, and other errors
- Searching the web archive is relatively fast
- The crawled to queued ratio approached ~0.7 at its maximum, meaning pages were being crawled relatively effectively
### Cons
- Parsing the first 500 characters after the `<body>` tag isn't always a great representation of the webpage's content due to aria labels, navigation components, and scripts
- Only crawls the first 500 tokens (open + closing tags) of a web page, which can result in an "incomplete" crawl of a page
- Ignores relative links, which can result in an "incomplete" crawl of a page
- The crawl speed decreases after about 500 seconds
