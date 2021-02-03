---
layout: post
title:  "COVID-19 Analysis: Humanity during Pandemic"
date:   2020-03-27 00:00:00 -0400
categories: analysis
---

<center><img src="https://raw.github.com/bmchardy/bmchardy.github.io/master/_posts/fleetwood-mac-tusk/wordart_2MB.png"></center>
<center>Tusk word cloud<br></center>

*Originally published on my Medium account [here](https://medium.com/@rjwmchardy/covid-19-analysis-humanity-during-pandemic-625ae0385ad3).*

**Disclaimer:** For reasons listed below (in Method), this graphic is *not* a good source of COVID-19 information and is instead meant to showcase human kindness in a time of crisis. For accurate and reliable sources, please see links below for Federal and International Authorities.

# Description

This graphic tracks the progression of the COVID-19 virus between January 23rd and March 22nd — up until this past Sunday. I highlighted several media articles throughout this period that stand out as positive messages or, often, as acts of kindness. The goal of this graphic is to show how people band together and support one another through a time of global crisis. This, in no way, is meant to downplay people’s fears and anxieties. This is a true struggle and crisis that all people on this earth are being faced with right now. Reach out to your friends, family, and supports in order to help them and yourself.

Country size represents number of active cases in the country. Country colour represents percent of confirmed cases that have recovered*.

*Country colour is represented on a logarithmic scale; log of ‘resolved cases to confirmed cases’.

**Also Read:** The New York Times has [a new article out today](https://nyti.ms/2UlDPHS) (March 27th). The article is in the spirit of my write-up and it’s worth a read :)

# Sections
* Description (above)
* Purpose
* Tools
* Method
* Take Care ❤
* Other Thoughts

# Purpose
After downloading the [publicly-available COVID-19 dataset](https://www.tableau.com/covid-19-coronavirus-data-resources) maintained by [Johns Hopkins University](https://www.jhu.edu/) and made available by Tableau in .hyper, .csv, and Google Sheet formats, updated daily, I had a look at the data and wanted to present it in a way that highlights the good side of humanity (I hope this is something accomplished with the above graphic.) The one positive dimension in the data was the recovery count.

I wanted to look at what people are doing to cope in this pandemic. Specifically, how are people acting prosocially? I wanted to look at kindness, helping, and positivity.

# What Tools I Used
* [COVID-19 Global Data](https://www.tableau.com/covid-19-coronavirus-data-resources)
* Tableau
* Excel
* Python 2.7
* [News API](https://newsapi.org/)
* [Natural Language Toolkit (NLTK)](https://www.nltk.org/)

# My Method
Below, I’ll lay out the process I followed to create this graphic.

After downloading the [publicly-available COVID-19 Global Data](https://www.tableau.com/covid-19-coronavirus-data-resources) (I chose the CSV file), I began exploring it a little bit in both Excel and Tableau.

I downloaded the dataset on March 23rd. This turned out to be very fortunate because the 23rd was the last day that Johns Hopkins included COVID-19 recoveries. In downloading this data now, you will no longer find the recovery measure (and as a result, you will no longer find the active cases count: a function of confirmed cases and recovered cases). Recoveries were removed from the data because the count [was not reliable](https://github.com/CSSEGISandData/COVID-19/issues/1250#issuecomment-602271179). This makes sense. It’s not pragmatic for government officials to stay in touch with every patient until they are recovered.

Even though it’s “fortunate” that I was able to obtain this data on the last day it was reported, it’s also slightly problematic; recovery counts have been removed due to the measure’s lack of reliability. Therefore, in viewing this graphic, *please take the recoveries reported with a grain of salt*. As a result of the recoveries being excluded, the graphic cannot be updated past March 22nd. The graphic itself is thus *out of date and simply does not reflect the current state of the pandemic*. Most notably, the United States has overtaken Italy as the #1 hub of COVID-19 globally. This is partially due to the Trump administration’s blatant ineptitude and Trump’s repeated down-playing of the crisis (for example, just last night, March 26th, Trump said on Fox News that he does not believe New York needs as many respirators as they are requesting).

I wanted to incorporate media articles into my graphic, so this is where I began. This is also where most of the work was done.

## Preparing the Media Articles

In order to find news on human kindness from these past several months, I enlisted the help of Python to mine some relevant articles.

An overview of the process. I used the News API, a tool to request news articles of a specific topic within a specified range of dates via HTTPS GET. I requested the range of February 26th to March 25th (the Developer edition of the API only allows requests of articles published within the last month). I used the key terms ‘kindness’, ‘good news’, and ‘coronavirus’. `m` and `d` are integers representing month and day respectively.

```
import requests
# ...
# prepare url
url = ('http://newsapi.org/v2/everything?'
         'q=kindness good news "coronavirus"&'
         'from=2020-' + str(m) + '-' + str(d) + '&'
         'to=2020-' + str(m) + '-' + str(d) + '&'
         'sortBy=popularity&'
         'apiKey=[My API Key]')
  # get data from url
  response = requests.get(url)
```

This gave me a JSON object, `response`, containing all articles found matching the query.

From here, I was able to extract article `title`, article `source` (ex. ‘Associated Press’), and article `url`.

Next, Natural Language Toolkit (NLTK) was used to extract article title sentiment. I used:

```
from nltk.sentiment.vader import SentimentIntensityAnalyzer
# prep sentiment analyis
si_analyzer = SentimentIntensityAnalyzer()
```

For each article in `response`:

```
# compute sentiment dict (containing 'pos', 'neg', 'neu' keys)
sentiment_analysis = si_analyzer.polarity_scores(title)
# compute article's one-dimensional sentiment score
sentiment = sentiment_analysis['pos'] - sentiment_analysis['neg']
```

Thus, sentiment is represented by a real number in the bounds -1.0 to 1.0; from completely negative to completely positive in sentiment.

From here, I exported a CSV, `data_news.csv`, containing columns `date`, `article_title`, `source`, `url`, and `sentiment`.

In Excel, I removed all sources that I considered non-reputable, hobbiest’s blogs, religious websites, or extreme right/left leaning news. Examples below.

*Allkpop.com, Alternet.org, Apracticalwedding.com, …, Breitbart News, …, Cupcakesandcashmere.com, …, Nakedcapitalism.com, …, Nintendolife.com, …, Wonkette.com*.

All articles with a sentiment below 0 were removed with two exceptions. All articles with a sentiment above 0 were kept with six exceptions.

For each date that had multiple articles, the article with the highest calculated sentiment was used while the rest were removed. There were some exceptions made to this rule because it was not always the highest sentiment article that was most relevant to this exercise.

At this point, I had no useful articles published between January 23rd and March 12th. I was wondering why this might be. There could be many reasons, but since there was no major western outbreak at this point, I’m guessing (pessimistically) that we just didn’t care yet. “It may have been seen as an eastern problem, not ‘ours’”. There’s no need for the western media to report positive stories about the outbreak if we’re not dealing with it yet (*insert upside-down smiley emoji here*)… These days, the media reports positive stories in a feeble attempt to keep us all calm.

To fix this data drought, I manually searched Google news using the criteria ‘kindness good “coronavirus”’. I used Google’s search tools to specify a date range of January 23rd to March 12th. After much searching, I was able to find a few articles that filled in the gaps. These articles were manually added to the `data_news.csv`. It’s true though; there was little in the way of positive COVID-19 coverage during this period and my graphic reflects that accurately.

All told, I had 11 articles to report.

## Putting the Graphic Together

In Tableau, I took the COVID-19 data from Johns Hopkins and I left-joined my news data.
My main Tableau sheet was created (as prominently displayed above lol) using a Tableau Treemap. The Label (text displayed on the graphic) makes use of the COVID-19 data’s `Country_Region` field. Box size is controlled by `SUM(CasesActive)`. Colour is controlled by `LOG(SUM([CasesRecovered])/SUM([CasesConfirmed])+1, 10)`. — a logartithm of `CasesRecovered` over `CasesConfirmed`.

I understand that reporting % cases recovered logarithmically can be misleading because countries in initial stages of recovery will be given greater weighting in their strides, whereas countries with prolonged recovery will appear a bit more stagnant, even if they are reporting recoveries at an equal rate.

The reason I reported % cases recovered logarithmically is because % recovery varies greatly as a function of country and all countries are at different stages of this epidemic right now; one is 80%+ recovered (China), while most are sub 20%.

<center><img src="https://raw.githubusercontent.com/bmchardy/bmchardy.github.io/master/_posts/covid-19-analysis-humanity/pctRecoveriesLogIllustration.png"></center>
<center><b>At top:</b> % Recover (Recoveries / Confirmed Cases). Notice the skew. <b>Below:</b> Log % Recoveries.<br></center>

When I first tried reporting the recoveries result linearly, I found China to be a *big* outlier (signalling strong progress), while most other countries were still in their early stages. This big discrepancy made China look like the only one to make progress and this is not what I was aiming for; other countries are all working hard. I therefore opted for a logarithmic colour function which shows the big strides toward recovery other countries are taking in better detail.

After all this, not much more needed to be done. I paged the Tableau Sheet by date. Then I put the vizualization Sheet together with the news Sheet on a Tableau dashboard and the result is what is shown above.

# Take Care Everyone!

Even though I’m highlighting kindness, helping, and positivity, I’m not trying to suggest that it’s butterflies and rainbows — we’re all in a serious position.

We need to make sure to take care of ourselves, our friends, and our loved ones. We can do this by checking in and by being responsible.

It’s so natural to feel anxious in a time like this. Know that it’s okay and that there’s help available.

**See Canada’s response to COVID-19 [here](https://www.canada.ca/en/public-health/services/diseases/coronavirus-disease-covid-19.html) and see the World Health Organization’s (WHO) response [here](https://www.who.int/emergencies/diseases/novel-coronavirus-2019).**

How do we stay appropriately informed on the situation while minimizing our day-to-day stress? My tips.

**Change the thought!**

* Be mindful of the news without ruminating over it. It’ll take practice, but you can try practicing [Radical Acceptance](https://www.psychologytoday.com/ca/blog/pieces-mind/201207/radical-acceptance) from Dialectical Behaviour Therapy by accepting this moment. “It is what it is” and “this moment is the result of many past events”.

**Make some simple behavioural changes!**

* Don’t start off the day by checking the news in a frenzy; schedule a 20 minute slot of time around lunch or in the evening to get your updates. These days, this is so much easier said than done as the news is everywhere…
* Introduce some simple guided relaxation meditation YouTube videos into your daily routine. They can be fun and rewarding. Or Yoga!
* Practice standard self-care every day.
* Help yourself and your loved ones at the same time by checking in and sending positive energy versus anxiety.

Stay safe, calm, and optimistic my friends!

Take care everyone ❤

# Other Thoughts

The [New York Times](https://www.nytimes.com) is providing their COVID-19 coverage to everyone for free throughout the pandemic. Might be worth a look.

Also...

I haven’t had a chance to take a look yet, but the New York Times (that I read and love!) [has open-sourced their COVID-19 data](https://www.nytimes.com/article/coronavirus-county-data-us.html) for every county of every state in the United States. I’ll probably be looking at that next haha