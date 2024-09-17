# Web-Scraping-With-5-Different-Methods: Including-Using-LLMs
Web scraping involves automating the process of extracting data from websites using programming or dedicated tools. It’s essential for tasks like market research, data analysis, content aggregation, and competitive intelligence. This method is efficient, saving time and enhancing decision-making by enabling businesses to analyze trends and patterns, making it a powerful way to gather valuable information from the web.

I’m eager to explore this topic, which captivates many. Apart from *BeautifulSoup, Scrapy, and Selenium*, have you thought about using LLMs for web scraping? It’s a fascinating approach that deserves your attention.

## Method 1: BeautifulSoup and Requests for Web Scraping
The first approach utilizes the widely-used *BeautifulSoup* and *Requests* libraries. These tools simplify the process of parsing HTML and navigating the structure of web pages. Here's an example Python code snippet.

```
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Step 1: Send a GET request to the specified URL
response = requests.get(url)

# Step 2: Parse the HTML content of the response using BeautifulSoup
soup = BeautifulSoup(response.text, 'html.parser')

# Step 3: Save the HTML content to a text file for reference
with open("imdb.txt", "w", encoding="utf-8") as file:
    file.write(str(soup))
print("Page content has been saved to imdb.txt")

# Step 4: Extract movie data from the parsed HTML and store it in a list
movies_data = []
for movie in soup.find_all('div', class_='lister-item-content'):
    title = movie.find('a').text
    genre = movie.find('span', class_='genre').text.strip()
    stars = movie.find('div', class_='ipl-rating-star').find('span', class_='ipl-rating-star__rating').text
    runtime = movie.find('span', class_='runtime').text
    rating = movie.find('span', class_='ipl-rating-star__rating').text
    movies_data.append([title, genre, stars, runtime, rating])

# Step 5: Create a Pandas DataFrame from the extracted movie data
df = pd.DataFrame(movies_data, columns=['Title', 'Genre', 'Stars', 'Runtime', 'Rating'])

# Display the resulting DataFrame
df
```

## Method 2: ScraPy for Web Scraping
Scrapy is a robust and versatile web scraping framework. The following code snippet demonstrates how to use *ScraPy*, with explanations provided in the comments.

```
# Import necessary libraries
import scrapy
from scrapy.crawler import CrawlerProcess

# Define the Spider class for IMDb data extraction
class IMDbSpider(scrapy.Spider):
    # Name of the spider
    name = "imdb_spider"
    # Starting URL(s) for the spider to crawl
    start_urls = ["https://www.imdb.com/list/ls566941243/"]
    # start_urls = [url]

    # Parse method to extract data from the webpage
    def parse(self, response):
        # Iterate over each movie item on the webpage
        for movie in response.css('div.lister-item-content'):
            yield {
                'title': movie.css('h3.lister-item-header a::text').get(),
                'genre': movie.css('p.text-muted span.genre::text').get(),
                'runtime': movie.css('p.text-muted span.runtime::text').get(),
                'rating': movie.css('div.ipl-rating-star span.ipl-rating-star__rating::text').get(),
            }
# Initialize a CrawlerProcess instance with settings
process = CrawlerProcess(settings={
    'FEED_FORMAT': 'json',
    'FEED_URI': 'output.json',  # This will overwrite the file every time you run the spider
})


# Add the IMDbSpider to the crawling process
process.crawl(IMDbSpider)
# Start the crawling process
process.start()
```
```
import pandas as pd

# Read the output.json file into a DataFrame (jsonlines format)
df = pd.read_json('output.json')

# Display the DataFrame
df.head()
```
Present the data in a dataframe:
```
import pandas as pd

# Read the output.json file into a DataFrame (jsonlines format)
df = pd.read_json('output.json')

# Display the DataFrame
df.head()
```
