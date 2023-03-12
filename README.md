![Twitter-scraping](https://user-images.githubusercontent.com/122904369/224550110-a416e166-0b4c-47f7-83c9-71926bf3411c.jpg)
What is Twitter Scraping?

Scraping is a technique to get information from Social Network sites. Scraping Twitter can yield many insights into sentiments, opinions and social media trends. Analysing tweets, shares, likes, URLs and interests is a powerful way to derive insight into public conversations. It is legal to scrape Twitter or any other SNS(Social Networking Sites) to extract publicly available information, but you should be aware that the data extracted might contain personal data.

How to Scrape the Twitter Data?

  Scraping can be done with the help of many opensource libraries like 
	
  1. Tweepy
  2. Twint
  3. Snscrape
  4. Getoldtweets3
  
Step 1

  Importing the libraries.
  
  As I have already mentioned above the list of libraries/modules needed for the project. First we have to import all those libraries. Before that check if the libraries are already installed or not by using the below piece of code.
  	
	!pip install ["Name of the library"]
	
If the libraries are already installed then we have to import those into our script by mentioning the below codes.

        import re
        import csv
        from getpass import getpass
        from time import sleep
        from selenium.webdriver.common.keys import Keys
        from selenium.common.exceptions import NoSuchElementException
        from msedge.selenium_tools import Edge, EdgeOptions

Step 2

        def get_tweet_data(card):
    """Extract data from tweet card"""
    username = card.find_element_by_xpath('.//span').text
    try:
        handle = card.find_element_by_xpath('.//span[contains(text(), "@")]').text
    except NoSuchElementException:
        return
    
    try:
        postdate = card.find_element_by_xpath('.//time').get_attribute('datetime')
    except NoSuchElementException:
        return
    
    comment = card.find_element_by_xpath('.//div[2]/div[2]/div[1]').text
    responding = card.find_element_by_xpath('.//div[2]/div[2]/div[2]').text
    text = comment + responding
    reply_cnt = card.find_element_by_xpath('.//div[@data-testid="reply"]').text
    retweet_cnt = card.find_element_by_xpath('.//div[@data-testid="retweet"]').text
    like_cnt = card.find_element_by_xpath('.//div[@data-testid="like"]').text
    
    # get a string of all emojis contained in the tweet
    """Emojis are stored as images... so I convert the filename, which is stored as unicode, into 
    the emoji character."""
    emoji_tags = card.find_elements_by_xpath('.//img[contains(@src, "emoji")]')
    emoji_list = []
    for tag in emoji_tags:
        filename = tag.get_attribute('src')
        try:
            emoji = chr(int(re.search(r'svg\/([a-z0-9]+)\.svg', filename).group(1), base=16))
        except AttributeError:
            continue
        if emoji:
            emoji_list.append(emoji)
    emojis = ' '.join(emoji_list)
    
    tweet = (username, handle, postdate, text, emojis, reply_cnt, retweet_cnt, like_cnt)
    return tweet
