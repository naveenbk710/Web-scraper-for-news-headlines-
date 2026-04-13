# Web-scraper-for-news-headlines-
import requests
from bs4 import BeautifulSoup

def scrape_headlines(url):
    headers = {
        "User-Agent": "Mozilla/5.0"
    }

    try:
        
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()

        
        soup = BeautifulSoup(response.text, "html.parser")

      
        headlines = soup.find_all(['h2', 'h3'])

        with open("headlines.txt", "w", encoding="utf-8") as file:
            count = 1
            for tag in headlines:
                text = tag.get_text().strip()
                if text:
                    file.write(f"{count}. {text}\n")
                    count += 1

        print("Headlines successfully saved to headlines.txt")

    except requests.exceptions.RequestException as e:
        print("Error fetching the website:", e)
    except Exception as e:
        print("Error processing data:", e)


if __name__ == "__main__":
    
    url = "https://news.ycombinator.com/"
    scrape_headlines(url)
