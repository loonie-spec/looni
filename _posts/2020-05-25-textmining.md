---
layout: post
title: A study on the text mining
---


**Hello!** I recently published the visual journalism project [그래프로 보는 이전과 달라진 한국](https://www.bbc.com/korean/news-52601647) in BBC’s Korean Service on 11 May. This post covers techniques and approaches applied to text mining and visualising the data.
{: .message }

![Screenshot 2020-05-25 at 12 39 53](https://user-images.githubusercontent.com/56850104/83526389-4aa14780-a4de-11ea-8a89-990eb64acdad.png)

Since a new coronavirus outbreak was reported in Wuhan on 31 December 2019 for the first time, the outbreak has been all over the media. My focus is on the Korean media’s coverage of the corona crisis and visualising it.

The process is broken down into three stages:

> 1 Web Scraping
> 2 Natural Language Processing
> 3 Visualising


### 1 Web Scraping

Using the news search function of Naver News<sup id="a1">[1]</sup> I collected news articles by keyword ‘Corona19(코로나19 in Korean)’ and by date from 20 Jan 2020 to 30 April 2020.

The search keyword Corona19 is a common name that Korean media call Covid-19. I set the date range from 20 January, the day the first confirmed case was announced on in Korea. Then, I scrapped the website every 10 days.

There are three ways to scrape news articles from Naver News on R:
rvest(R package), Naver Open API, N2H4<sup id="a2">[2]</sup> (R package)

‘rvest’ is the most reliable package I can use to crawl the website for a specific date range.


### 2 Natural Language Processing

The first step to analyse articles was to break down the text into separate sentences. In this study, the Korean morphological analyser KoNLP was used for morpheme analysis. KoNLP is the only Korean natural language processing toolkit on R. With the SimplePos22 function of KoNLP I was able to extract nouns from the news articles and POS tagging.

The most complex part was identifying stop words. KoNLP can flag and filter them out by checking a hardcoded list of known stop words, as in the code below.
I could also use the buildDictionary function to add stop words to delete. Since I didn’t need “coronavirus infection(코로나바이러스 감염증)”, I added them into the dictionary let delete them.


``` 
add\_words <- c("코로나바이러스 감염증")
buildDictionary(user\_dic = data.frame(add\_words, rep("ncn", length(add\_words))), replace\_usr\_dic = T)
```



### 3 Visualising

To explore the most frequently used top 10 keywords by period I created an interactive bubble chart with the drop-down list for filtering date ranges.

Normally word cloud are used to visualise words that appear the most frequently in the source. But my intention is compared keywords by period. The bubble chart with the drop-down list does the job better in such a case.

[This is an English ver. bubble chart](https://public.flourish.studio/visualisation/2576893)
<div class="flourish-embed flourish-hierarchy" data-src="visualisation/2576893" data-url="https://flo.uri.sh/visualisation/2576893/embed"><script src="https://public.flourish.studio/resources/embed.js"></script></div>



I created interactive bubble charts as well. You can see how I created them when you click the links below. 
Which one do you like the most?

[packed bubble chart](https://codepen.io/looniii/pen/KKVPYQx)
![bs](https://user-images.githubusercontent.com/56850104/83526120-de264880-a4dd-11ea-847b-a2a03b9c6c0a.png)


[split packed bubble chart](https://codepen.io/looniii/pen/YzwKMLp)
![bb](https://user-images.githubusercontent.com/56850104/83526293-22194d80-a4de-11ea-9915-85187f6a4ade.png)



### One more thing 

Translating is an extra step in workflow for sharing the dataset with colleagues and tutors who speak different languages.

My initial plan was extracting more than 200 keywords each period to compare the main content of the news articles. Google translation API is a good option to translate a large amount of text.

I contacted engineers involved in making KoLNP to find a suitable machine translation for the Korean language.
Chel-Hee Lee, who is an adjunct assistant professor of University of Calgary in Canada, said;
“It may be a good idea to use GoogleLanguageR to translate into English. However, it is necessary to translate manually. Translation is a subjective matter at the end.”
He also said that natural language processing is a highly subjective matter.

I agree with him. Tools make you life easier, but you have to know how to use them. 



---
[1] Naver is the leading portal site in Korea. Its news service Naver News(http://news.naver.com) is a news aggregator website that takes a large portion of news consumption in Korea. It currently sources content from 52 news outlets in real-time. The site stores the articles on its database and presents all of them on its website. It means being able to read all news articles in real-time from all major news outlets on one website in one standardised format. Therefore, it is the place for scraping news articles. 

[2] N2H4 is the R package for Naver News Text Crawling. For more information, visit the website(https://github.com/forkonlp/N2H4)
