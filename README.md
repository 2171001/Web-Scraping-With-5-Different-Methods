# Web-Scraping-With-5-Different-Methods: Including-Using-LLMs
Web scraping involves automating the process of extracting data from websites using programming or dedicated tools. It’s essential for tasks like market research, data analysis, content aggregation, and competitive intelligence. This method is efficient, saving time and enhancing decision-making by enabling businesses to analyze trends and patterns, making it a powerful way to gather valuable information from the web.

I’m eager to explore this topic, which captivates many. Apart from ***BeautifulSoup, Scrapy, and Selenium***, have you thought about using LLMs for web scraping? It’s a fascinating approach that deserves your attention.

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

## Method 3: Selenium for Web Scraping
Selenium is commonly utilized for scraping dynamic websites. Below is a simple example:
```
from selenium import webdriver
from bs4 import BeautifulSoup
import pandas as pd

# URL of the IMDb list
url = "https://www.imdb.com/list/ls566941243/"

# Set up Chrome options to run the browser in incognito mode
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("--incognito")

# Initialize the Chrome driver with the specified options
driver = webdriver.Chrome(options=chrome_options)

# Navigate to the IMDb list URL
driver.get(url)

# Wait for the page to load (adjust the wait time according to your webpage)
driver.implicitly_wait(10)

# Get the HTML content of the page after it has fully loaded
html_content = driver.page_source

# Parse the HTML content with BeautifulSoup
soup = BeautifulSoup(html_content, 'html.parser')

# Save the HTML content to a text file for reference
with open("imdb_selenium.txt", "w", encoding="utf-8") as file:
    file.write(str(soup))
print("Page content has been saved to imdb_selenium.txt")

# Extract movie data from the parsed HTML
movies_data = []
for movie in soup.find_all('div', class_='lister-item-content'):
    title = movie.find('a').text
    genre = movie.find('span', class_='genre').text.strip()
    stars = movie.select_one('div.ipl-rating-star span.ipl-rating-star__rating').text
    runtime = movie.find('span', class_='runtime').text
    rating = movie.select_one('div.ipl-rating-star span.ipl-rating-star__rating').text
    movies_data.append([title, genre, stars, runtime, rating])

# Create a Pandas DataFrame from the collected movie data
df = pd.DataFrame(movies_data, columns=['Title', 'Genre', 'Stars', 'Runtime', 'Rating'])

# Display the resulting DataFrame
print(df)

# Close the Chrome driver
driver.quit()
```
The key to using Selenium effectively lies in Chrome options, which allow you to customize the behavior of the Chrome browser when it's controlled by Selenium WebDriver. These settings let you manage features like incognito mode, window size, notifications, and more.

Below are some essential Chrome options you may find helpful:
```
# Runs the browser in incognito (private browsing) mode.
chrome_options.add_argument("--incognito")

# Runs the browser in headless mode, i.e., without a graphical user interface. 
# Useful for running Selenium tests in the background without opening a visible browser window.
chrome_options.add_argument("--headless")

# Sets the initial window size of the browser.
chrome_options.add_argument("--window-size=1200x600")

# Disables browser notifications.
chrome_options.add_argument("--disable-notifications")

# Disables the infobar that appears at the top of the browser.
chrome_options.add_argument("--disable-infobars")

# Disables browser extensions.
chrome_options.add_argument("--disable-extensions")

# Disables the GPU hardware acceleration.
chrome_options.add_argument("--disable-gpu")

# Disables web security features, which can be useful for testing on localhost without CORS issues.
chrome_options.add_argument("--disable-web-security")
```
You can use these options individually or in combination, depending on your needs. When initializing a **webdriver.Chrome** instance, pass these options through the **options** parameter:
```
from selenium import webdriver

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("--incognito")
chrome_options.add_argument("--headless")

driver = webdriver.Chrome(options=chrome_options)
```
These options offer flexibility and control over the browser's behavior when using Selenium for web automation or testing. Select the options that best suit your specific use case and needs.

## Method 4: Requests and lxml for Web Scraping
lxml is a Python library that integrates with the C libraries libxml2 and libxslt, offering the speed and functionality of XML with a straightforward native Python API. It is similar to ElementTree but provides additional advantages.
```
import requests
from lxml import html
import pandas as pd

# Define the URL
url = "https://www.imdb.com/list/ls566941243/"

# Send an HTTP request to the URL and get the response
response = requests.get(url)

# Parse the HTML content using lxml
tree = html.fromstring(response.content)

# Extract movie data from the parsed HTML
titles = tree.xpath('//h3[@class="lister-item-header"]/a/text()')
genres = [', '.join(genre.strip() for genre in genre_list.xpath(".//text()")) for genre_list in tree.xpath('//p[@class="text-muted text-small"]/span[@class="genre"]')]
ratings = tree.xpath('//div[@class="ipl-rating-star small"]/span[@class="ipl-rating-star__rating"]/text()')
runtimes = tree.xpath('//p[@class="text-muted text-small"]/span[@class="runtime"]/text()')

# Create a dictionary with extracted data
data = {
    'Title': titles,
    'Genre': genres,
    'Rating': ratings,
    'Runtime': runtimes
}

# Create a DataFrame from the dictionary
df = pd.DataFrame(data)

# Display the resulting DataFrame
df.head()
```

## Method 5: How to Use LangChain for Web Scraping
I'm sure most of you reading this are familiar with tools like ChatGPT and BARD. Large Language Models (LLMs) simplify many tasks. Whether you're asking "Who is Donald Trump?" or requesting a translation from German to English, they provide quick answers. But did you know you can also use them for web scraping? Here's how.
👉Here are some useful LangChain resources for this demo:
* [LangChain Beautiful Soup](https://python.langchain.com/docs/integrations/document_transformers/beautiful_soup)
* [LangChain Extraction](https://python.langchain.com/v0.1/docs/use_cases/extraction)

