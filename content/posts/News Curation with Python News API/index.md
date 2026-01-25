---
title: News Curation with Python News API
---
I've found myself feeling disconnected from current events for a few years now and I've been trapped in the loop of wanting to find news sources I can trust to read from regularly, but then find myself just skimming a hundred different sources because I'm not sure what to read.

I thought using an API to request json object news articles would help me more effectively sift through the data to find what I want. I would prefer to consume a mix of financial, tech, science, and political news

Using this News API at: newsapi.org you can get a free API key in minutes

News API allows you to filter your results in a variety of ways, so I need to decide on the sources I'd like to scan articles from.

This News API will not produce results for searches that are too broad, and the criteria for 'too broad' seems to be tied to the number of sources 

I first look through the different sources I have available:

```
def printSources() -> list:
	data = newsapi.get_sources()
	for source in data['sources']:
		print(source['name'])
		
OUTPUT:
------------------------
ABC News
ABC News (AU)
Aftenposten
Al Jazeera English
ANSA.it
Argaam
Ars Technica
Ary News
Associated Press
Australian Financial Review
Axios
BBC News...
```


This returns a list of officially supported sources

You can also set specific domains to use as a filter in the get_everything() and get_top_headlines()

So I can either search by article author, source id, source name, source domain, and more to get more selective with my media. 

I decided on a small number of domains and keywords for daily searches 

I initially made a mistake in the logic of searching with keywords on each domain that resulted in the same article having duplicates.

```
for article in data['articles'][:5]:
    if f"{article['url']}\n" in usedlinks:
        pass

    else:
        email += f"""====================\nTitle: {article['title']}\nAuthor: {article['author']}\nDescription: {article['description'][:80]}\n{article['url']}====================\n"""
        
        print(article)
        links_just_used.append(article['url'])

with open('usedlinks.txt', 'a') as file:
    for link in links_just_used:
        file.write(link + "\n")

```

This was fixed by removing duplicates from the list before appending its data to the docstring. Since preserving the order of the list is not necessary then we can achieve this quickly by turning the list into a set and then back into a list. 

I also changed the data structure in the final version by adding unique articles to a dictionary and adding that dictionary to a list while in the loop. This removes the need to remove duplicates as duplicates would never make it past this formatting step.

I request data on a daily basis at noon since more articles have been written later in the day, and the I send them to my own email in a fancy docstring with working hyperlinks through the python SMTP library

This works like a charm and the only way I see myself improving on this is by generating HTML & CSS and using an article image URL to make the email look more pretty, which I may do soon as it sounds cool.

