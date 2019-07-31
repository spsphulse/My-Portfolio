---
layout: post
title: 'Whats going on, Amazon?'
published: true
---

Last year, one of my broken earphones led me to a notorious bot culture on Amazon. I even wrote a Linkedin [article](https://www.linkedin.com/pulse/you-shop-amazon-absolutely-need-read-swapnil-phulse/) about it. It received decent attention at the time. I thought about expanding on the project and doing similar analysis for other low-cost products. But as always, life happened, other things took over and I completely forgot about it.

**Fast forward to last weekend:**  Recently I have been overwhelmed with multiple projects (at work as well as my own side projects). So I was looking for some way to manage everything efficiently. After finding that **'Getting Things Done(GTD) by David Allen'** was mentioned in multiple HN discussions as well as reddit posts on a few self-help subs, I decided to buy it on Amazon. Here I just stumbled onto some really really odd reviews. This post is about that.

![GTD book on amazon](https://raw.githubusercontent.com/spsphulse/My-Portfolio/master/images/amz_gtd_images/1.PNG)


Typically, when I choose to buy books on Amazon, I look at both _Top reviews_ as well as _Most recent reviews_. Reasons are two-folds:
1. Top reviews obviously speak for most of the people and give you a general sense if the book is really good or fluff(especially in this genre of productivity/self-help books)
2. Recent reviews to check if the book still hold relevance. A bunch of times I have found people complaining how recent edition of a book doesn't live up to expections. In that case, I'd rather buy the original print.
    
Hence, I went over to check recent reviews for GTD on amazon and was surprised to see the reviews. Here is a screenshot I took.

![Recent reviews as of 07/27 morning](https://github.com/spsphulse/My-Portfolio/blob/master/images/amz_gtd_images/gtd_recent_reviews.png?raw=true)


A couple of interesting things to note:
- Barring that one review highlighted in green, all other reviews do not seem to be related to the GTD book( or any book really)
- Most of the reviews seem to mention about a hose or a flag.

That was it! That was enough for the serial procrastinator in me to abandon all of my other ongoing projects and start with this new analysis of reviews for _Getting Things Done_. Do you notice the irony here? Anyhoo, I promised myself I won't spend more than a couple hours on this. So the code is mostly spaghetti. I just wanted to find some odd reviews & note down my cusrsory findings. Good enough topic for a first blog post,eh?

## Part 1 Scraping
I started by quickly scraping all the amazon reviews for GTD. Just used Python, Beautifulsoup & Requests. Nothing fancy. It consists of:
1. Finding total number of reviews for the book
2. Deciding number of review pages(total_reviews//10+1) and creating URLs to parse
3. For each URL, pare the reviews therein, create a final dataframe and store these reviews.

Github repo for this has been shared at the end.

## Part 2 Data processing and finding most frequent words
I did some preliminary cleaning of the reviews, chose the most recent 200 reviews and plotted the most commonly occuring words/tokens. The chart is shown below

![Frequest works from most recent 200 reviews](https://github.com/spsphulse/My-Portfolio/blob/master/images/amz_gtd_images/frequent_words.png?raw=true)

Right off the bat, a few words that are highlighted in red jumped out as odd.What is hose,water and flag doing in a 'book' review.



## Part 3 Topic Modelling
Quite recently I had read a few articles on topic modelling using NLP, but hadn't used it yet. I figured that could come in handy here to find topics that are not really associated with book/reading a book. That was my next & final step.

1. First I created a corpus from the collection of book reviews
2. Then I converted it to a document matrix using BOW(bag of words) approach in gensim. 
3. After that I trained an LDA(Latent Dirichlet allocation) model using this document matrix. 
4. To understand a bit better, I visualized the results using pyLDAvis library. 

Sharing some odd ones that I found over here.

![LDA viz 1](https://github.com/spsphulse/My-Portfolio/blob/master/images/amz_gtd_images/ldavis1.PNG?raw=true)

![LDA viz 2](https://github.com/spsphulse/My-Portfolio/blob/master/images/amz_gtd_images/ldavis2.PNG?raw=true)

![LDA viz 3](https://github.com/spsphulse/My-Portfolio/blob/master/images/amz_gtd_images/ldavis3.PNG?raw=true)

Especially stood out to me were reviews with words like
- Hose
- Flag
- Metal
- Water
- Ponytail
- Coffee
- Nozzle
- Sturdy
- Barn
- Garden
- Plant


Alright, finally just for funsies, I removed majority of the reviews indicating a book (remove reviews containing common words indicating a book review such as david,author,audible,GTD etc) and recreated the LDA model to check odd reviews within that subset (shared result at the end for one of the clusters)


I think Amazon can do a better job of curating the reviews on their products.

**Reactively**: Find out who fucked up? Was it Ben, the new intern? How else did we end up with such irrelevant reviews? Or are the sellers back at it again bloating review numbers with just trash?

**Proactively**: How about an advanced topic model or an advanced _'insert some advanced deep learning NLP stuff like BERT,XLNet'_ model that is incrementally run on new reviews, identify irrelevant topics and remove the reviews that don't seem to make sense.

Because otherwise, how do I rely on that number of reviews(3176 in this case) and utimately the rating. Amazon must be using 'number of reviews' to come up with various metrics that drive where this book gets featured(1st page, 2nd page etc).Some of them are clearly not at all related to the book. The bot culture I mentioned at the very beginning is another problem for them to handle altogether. I believe a company such as Amazon definitely has the smarts as well as the resources to do this at their scale.

There are so many improvements that can be done with feature engineering and well thought validation scheme on this code itself.Some other day perhaps. Right now I gotta read & apply my GTD book - then start finishing my actual ongoing projects, right? For others willing to dig in, I've put up my half-assed github repo [here](https://github.com/spsphulse/Amazon-Getting-Things-Done-odd-reviews). If you only need data to play around with, it is [here](https://github.com/spsphulse/Amazon-Getting-Things-Done-odd-reviews/blob/master/GTD_Final_Reviews.csv).

Until next time! Lastly, just wanted to display contents of one of the LDA clusters of reviews here at the end. Have fun going through it. Theres all kinds of weird things - including capes, cakes & diarrhea (*giggles*).


*********************************************************************************************************

"Well Made !"

"Love this shirt, and it came in just in time for the finale!!  Fits as expected, and great quality!"

"I have used this hose one time an it seems to be a very good an strong hose. I am very satisfied."

"Hose was exactly as expected."

"Muy buen libro."

"I like that it's big enough to fit me properly. I am using it post surgical and helps so much more than what the hospital sends you home with."

"I can’t get the stud to fit or stay in place so I can pierce. I haven’t been able to use it at all."

"Has storage and hold water, light weight"

"These were exactly what I needed.  They work really well on anything I have placed them on.  My grandson applied them to his shotgun & rifle. A good value."

"They are just as advertised"

"He never met an adjective he couodn’t throw a handful of synonyms at alongside. Gets pretty fatiguing."

"The earings arent sharp and it didn't pierce"

"Was exactly what I wanted"

"It arrived as advertised"

"Looove this! Just what I needed!"

"It is not what we expected - they are to flat"

"Great fits perfectly"

"Good cd, very helpful"

"As described, fits perfectly"

"Totally worth your time"

"is an excellent product if you want to reduce sizes thanks to its excellent design I recommend it widely because it gives very good results, it is the best investment."

"Hi not what i expect aorry i returned"

"Bought this for a Caribbean cruise last summer. It kept my water cold & ice intact for 48 hours. Unbelievable! Continued to use it at work for my 16-hour overnight shifts. Just last week, it stopped keeping the water cold & the ice melted quickly. Sent an email to Healthy Human & they responded immediately. Replacing it immediately!  I am more than satisfied with this purchase & will continue to recommend it to anyone & everyone."

"wonderful chocolate flavor,"

"This is an amazing vest! It really makes me sweat during workouts and it came packaged nicely. It was true to size for me and fit perfectly.. I wear a medium in clothes and bought a medium."

"What I like about this product is it's smooth non-fishy taste."

"The utensils are very good and durable"

"Amazing insights into the nitty gritty of managing your all inclusive stuff to get my mind clear!!!!"

"This is amazing.The first day I put on the sauna suit I went walking. When I came back it was soaked. I was very surprised so Ive been wearing it everytime I walk! The top part is a little tricky to get into as it has a zipper. I am a 36ddd and about 220. I have a pear shape so the velcro on the tummy part is great!  Its been about 6 days of walking and I see results!! Not huuuge results but enough I notice my shirt around my tummy fits differently."

"It is mostly common sense stuff, but good for making you think about it all."

"I got this block and not the tablets I expected.  I returned it"

"I did not buy this sorry"

"great tips!  helpful in small and big ways"

"ILove it !!!!!!!!"

"Even nicer in person"

"The topper was exactly as pictured and the birthday girl loved it! Shipped on time too, as it was ordered very last minute and it arrived the next day!"

"Holds everything very good for the gym which I needed it for and works great under clothes too"

"love it fit good"

"Really like it"

"Perfect for cross fit!"

"Absolutely love it!"

"They are heavy"

"Received item as pictured, very beautiful and my daughter was happy with her cake topper. Quality is very good, fast delivery and would definitely recommend."

"Exactly as advertised and exactly what I needed."

"Works well. Well made"

"These work perfect, love the feel! Highly recommend to anyone wanting to lift without injuring their hands."

"Best seller ever. Amazing people run the company. High recommended"

"Grandchildren loved the capes and masks. Great for creative play."

"Exactly what I wanted !"

"Had hoped it would be brighter but it will do what I purchased it for"

"Now I can stop signing it out from the Library!!"

"I got it and I like it! Gonna buy another!!"

"Exactly what I thought I was buying, unlike other things I've bought on Amazon!"

"Never ordered this"

"My nephew loved the capes! Thank you!!"

"Very nice rhinestone cake numbers.  Exactly as described.  Shipped very quickly."

"I loved it!"

"I loved it"

"just what I need"

"He has some helpful charts but the prose is long and sloggy for me.  Hard to get through to the efficient charts."

"Just like I expected!!!"

"I have been satisfied with all of my purchases"

"Waited until last minute to order something and this made the stress worth it! Great weight and was a hit on the cake."

"I didnt buy this"

"Sent it to my nephew I'm the best aunt ever👍👍👍👍"

"Absolutely love it"

"I feel better physically since started using them."

"Great product! Had issues with gas/diarrhea at least twice a week. Once I began taking this probiotic, the problem has diminished considerably. I highly recommend it!"

"So far so good...will update after a few weeks."

"Great function and design. The magnet STICKS!!"

"She loved it"

"Pretty much:1. Think it2. Do it"

"They are perfect!"

"Just what we were looking for"

"Yes, worked perfectly!"

"I absolutely recommend it."

"Set it aside and have't gotten back to it."

"Exactly what I wanted."

"I just returned from a week long trip. Used I Travel Well during my time away. I believe it helped with flying, different altitude, temperature change, eating differently, etc. transition home was very smooth."

"Great for my son's toys. Low enough for him to reach into."

"Look no further."

"Great stuff works fabulously try it"

"Second bottle of this product. Ran out of first a few weeks ago. Was really pleased with overall appearance in texture, luminosity and general tightening of pores resulting from continued use. As soon as I received my second bottle I applied product.  Was pleasantly surprised next morning. Noticeable improvement. Husband even noticed by the third day of use. Incredible value. Especially for results."

"Mostly satisfied with this product. The phone pocket is a little tight for my S5 in an otter box case."

"It was not as helpful as I expected.  Not real easy to listen to"

"I never ordered this."

"Exactly as described."

"can't wait to try"

"Love it. Tried it on my hands and they were soft for hours.  Subtle coconut scent which is nice.  Highly recommend"

"Epic - Knowledge at its best !"

"Excellent. So helpful!"

"This made me twice as productive and is really actionable."

"Incredibly helpful. A must for anyone who wants to get more done, and still be a balanced, normal human being."

"Pure wisdom. Very recommended."

"The basic idea is just what I have been looking for. I did not need the scientific reasoning behind everything and could have used a little more specifics on method"

"Ho hum! I did not find this helpful."

"Great advice and tips, very helpful."

"Delivered very fast! Exactly as described"

"Good stuff. Most useful."

"well-shipped, as described"

"Highly recommended to every one"

"good condition, very helpful"

"Was not what I expected!"

"See title ;)"

"Lives up to all the hype."

"Very helpful, shared it with my grandson and daughter....."

"Very helpful. Confirmed some of my ideas and added several valuable new ideas."

"Incredibly simple yet truly life changing process. It's amazing how much more productive and organized I am. I will recommend this to everyone I know!"

"The concepts are good, however it needs to be made current with today's technologies.  When was this last time you used a Palm Pilot?"

"Great ideas and solutions.  Just takes personal discipline to apply and stick with it.  I really like the 2 min. rule."

"very out of date for todays world.  You can apply some principles but many will not apply in today tech world"

"it was very informative...especially the two minute instructions....if it only takes two minutes to do then do it...that really works...lol"

"Just the clarity and coaching I was hoping for, even more insight than I could have imagined."

*********************************************************************************************************
