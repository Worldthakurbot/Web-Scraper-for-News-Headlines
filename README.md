# Web-Scraper-for-News-Headlines

import http.client

from html.parser import HTMLParser

class HeadlineParser(HTMLParser):

    def __init__(self):
        super().__init__()
        self.recording = False
        self.headlines = []

    def handle_starttag(self, tag, attrs):
        if tag in ("h1", "h2", "h3"):
            self.recording = True

    def handle_endtag(self, tag):
        if tag in ("h1", "h2", "h3"):
            self.recording = False

    def handle_data(self, data):
        if self.recording and data.strip():
            self.headlines.append(data.strip())

# Step 1: Connect to website and fetch HTML
conn = http.client.HTTPSConnection("www.bbc.com")

conn.request("GET", "/news")

response = conn.getresponse()

html = response.read().decode()

# Step 2: Parse HTML to extract headlines
parser = HeadlineParser()

parser.feed(html)

# Step 3: Save to .txt file
with open("headlines_builtin.txt", "w", encoding="utf-8") as file:

    for headline in parser.headlines:
    
        file.write(headline + "\n")

print("✅ Headlines saved to 'headlines_builtin.txt'")


