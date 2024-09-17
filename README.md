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
In each web scraping approach, I save the HTML to an imdb.txt file for a clearer view of the specific HTML elements being targeted.
![image](https://github.com/user-attachments/assets/6c169635-e68f-430b-b042-53860192111d)
Final Output:
![image](https://github.com/user-attachments/assets/13b0e193-668b-4209-9a22-a0dcbe149049)
