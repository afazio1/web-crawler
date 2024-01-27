# CS 4675 Web Crawler

## Overview
For HW1, I decided to choose Option 1.2 and write a web crawler of my own. This web crawler is written in Go and uses MongoDB as a web archive. The crawler can be run locally without access to the web archive. 

![Command Line Output](https://github.com/afazio1/web-crawler/blob/main/images/cmd-ui.png)
## How it works
Here's the the data flow of the web crawler:
![Web Crawler Diagram](https://github.com/afazio1/web-crawler/blob/main/images/web-crawler-diagram.png)

## Web Archive
To store the content of crawled web pages, I used MongoDB, a NoSQL database with a built-in Search Index functionality. 

I decided to store the `url`, `title`, and first 500 characters after the `<body>` tag as `content`. Then, I created a Search Index on the `title` and `content` fields, using MongoDB's standard keyword analyzer. This created an inverted index table which mapped keywords to webpages. 

The Search Tester GUI allowed me to query the web archive.
![Searching for master's in computer science](https://github.com/afazio1/web-crawler/blob/main/images/web-archive.png)

## Performance
In my program, I used Go's built-in Time library to record the Crawl Statistics every minute. The statistics included:
- Crawl Speed: Pages / second
- Crawled to Queued Ratio / second

![Crawl speed graph](https://github.com/afazio1/web-crawler/blob/main/images/crawl-speed-graph.png)

![Crawl ratio graph](https://github.com/afazio1/web-crawler/blob/main/images/crawl-ratio-graph.png)
## Experience
I chose to use simple technologies for the web crawler in order to fully understand all the components of the system. The only external services/libraries I used were MongoDB Atlas Search for the implementation of the searchable web archive. All other operations including fetching webpages, parsing HTML, threading, and benchmarking were done using Go's standard library. With this approach, I prioritized simplicity which will allow me to easily expand on this software in the future.

Although my web crawler accomplished the goal of crawling at least 1000 pages, here are a few future enhancements I could make. First, I would like to allow the user to decide the seed url. This could be easily implemented in my command line tool by prompting the user for a url at the start of the program. However, there would need to be some added error handling for if a user enters an invalid url. 

Currently, the crawler searches Breadth-First. I would like to be able to experiment with different crawling algorithms like BFS, DFS, and hybrids. Making a swappable crawl would be ideal to compare the crawler statistics.

Below is a summary of the pros and cons of my web crawler.

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



