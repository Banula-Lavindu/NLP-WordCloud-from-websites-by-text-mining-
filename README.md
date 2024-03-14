
# Website Word Cloud Generator

This Python script fetches content from a website, analyzes its word frequencies, and generates a word cloud visualization.

## Usage

1. Clone the repository:
   ```bash
   https://github.com/Banula-Lavindu/NLP-WordCloud-from-websites-by-text-mining-.git
   ```
2. Navigate to the project directory:
   ```bash
   cd website-word-cloud
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Run the script:
   ```bash
   python generate_word_cloud.py
   ```

## Description

This script extracts text content from a specified website, tokenizes it, and calculates word frequencies. It then generates a word cloud to visualize the frequency distribution of words present in the content.

## Example

For demonstration, let's generate a word cloud for the official website of the Government of Sri Lanka (https://www.gov.lk/).

```python
# Fetching Website Content
import urllib.request

response = urllib.request.urlopen('https://www.gov.lk/')
html = response.read()
html_content = html.decode('utf-8')

# Tokenization
import re
tokens = re.split('\W+', html_content)

# Cleaning HTML Content
from bs4 import BeautifulSoup

soup = BeautifulSoup(html_content, 'html.parser')
clean_text = soup.get_text()
tokens = [tok for tok in clean_text.split()]

# Calculating Word Frequencies
freq_dist = {}
for tok in tokens:
    if tok in freq_dist:
        freq_dist[tok] = freq_dist[tok] + 1
    else:
        freq_dist[tok] = 1

# Plotting Word Frequencies
import nltk
freq_dist_nltk = nltk.FreqDist(tokens)
freq_dist_nltk.plot(30, cumulative=False)

# Removing Stopwords
from nltk.corpus import stopwords

stopword_list = set(stopwords.words('english'))
clean_tokens = [tok for tok in tokens if len(tok.lower()) > 1 and (tok.lower() not in stopword_list)]

# Generating Word Cloud
from wordcloud import WordCloud
import matplotlib.pyplot as plt

word_freq_dict = {word: freq for word, freq in freq_dist_nltk.items()}
word_cloud = WordCloud(width=800,
                       height=800,
                       background_color='white',
                       min_font_size=10)
word_cloud.generate_from_frequencies(word_freq_dict)

# Displaying Word Cloud
plt.figure()
plt.imshow(word_cloud)
plt.axis('off')
plt.show()
```

## Dependencies

- Python 3.x
- BeautifulSoup
- NLTK
- Matplotlib
- WordCloud

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

