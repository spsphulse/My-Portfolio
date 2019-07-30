---
layout: post
title: 'Whats going on, Amazon?'
published: true
---

Last year, one of my broken earphones led me to write a Linkedin article when I had stumbled onto a notorious bot culture on Amazon. It received decent attention at the time. I thought about expanding on the project and doing similar analysis for other low-cost products. But as always, life happened, other things took over and I completely forgot about it.

I have been overwhelmed recently with multiple projects (at work as well as my own side-gigs). So I was looking for some way to manage everything. After finding that **'Getting Things Done(GTD) by David Allen'** was mentioned in multiple HN discussions as well as reddit posts, I decided to buy it on Amazon.

Typically, when I choose to buy books on Amazon, I look at both _Top reviews_ as well as _Most recent reviews_. Reasons are twofolds:
1. Top reviews obviously speak for most of the people and give you a general sense if the book is really good or fluff(especially in this genre of productivity/self-help books
2. Recent reviews to check if the book still hold relevance. A bunch of times I have found people complaining how recent edition of a book doesn't live up to expections. In that case, I'd rather buy the original print.

    
Hence, I went over to check recent reviews for GTD on amazon and was surprised to see the reviews. Here is a screenshot I took.

A couple of interesting things to note:
- Barring that one review highlighted in green, all other reviews doesn not seem to be about the GTD book( or any book really)
- Most of the reviews seem to mention about a hose or a flag.

That was it! That was enough for the serial procrastinator in me to abandon all of my other ongoing projects and start with this new analysis of reviews for GTD on Amazon. Do you notice the irony here? Anyhoo, I promised myself I won't spend more than a couple hours on this. I just wanted to note down my cusrsory findings. Good enough topic for a first blog post,eh?

## Part 1 Scraping
I started by quickly scraping all the amazon reviews for GTD. Just used Python, Beautifulsoup & Requests. Nothing fancy. It consists of:
1.Finding total number of reviews for the book
2.Deciding number of review pages(total_reviews//10+1) and creating URLs to parse
3.For each URL, pare the reviews therein, create a final dataframe and store these reviews.

Jupyter notebook for the same can be found here.

## Part 2 Data processing and finding most frequent words
I did some preliminary cleaning of the reviews, chose the most recent 100 reviews and plotted the most commonly occuring words/tokens. The chart is shown below


Right off the bat, a few words that are highlighted in red jumped out as odd.Quite recently I had read a few articles on topic modeeling using NLP, but hadn't used it yet. I figured that could come in handy here to find topics that are not really associated with book/reading a book. That was my next & final step.


## Part 3 Topic Modelling
First I created a corpus and converted it to a document matrix using BOW(bag of words) approach in gensim. After that I trained an LDA(Latent Dirichlet allocation) model using the doc matrix. To understand a bit better, we visualize the results using pyLDAvis library. Especially stood out reviews with words like
-Hose
-Flag
-Metal
-Water
-Ponytail
-Coffee
-Nozzle
-Sturdy
-Barn
-Garden
-Plant


Finally just for funsies, I removed majority reviews indicating the GTD book using the following code(containing words such as david,author,GTD etc).

```python
df = pd.read_csv('GTD_Final_Reviews.csv',index_col=None)

book_indicators=['book','gtd','productivity','audible','david','allen','tasks','read',
                 'write','author','boring','information','system','plan','focus',
                'schedule','practice','busy']

#~(df.Comment.apply(lambda review: any(word in review for word in book_indicators)))
df = df[(~df.Comment.isnull())]
df = df[(~(df.Comment.apply(lambda review: any(word in review.lower() for word in book_indicators))))]
df.shape
```

Then I built an LDA model and just listed one of the cluster content. I think Amazon can do a better job of suprvising the comments on their products. Because otherwise, for example, how do I rely on that number of bloated reviews which are not at all related to the book. 

Just wanted to display one of the cluster contents here at the end. Have fun going through it. There's all kinds of weird things including hysterectomy(giggle giggle!)

```python

['We love this product, easy to use, very fast expand and seems good quality.',
 'Gaarden',
 'Love Barstool Sports. when i saw the merch on Amazon i had to buy. Fast shipping as usual. Will be buying more.',
 'It would be great if i received it, it is saying it was delivered to me...i got everything in the order except that...I’m not sure how to go about this...i have never experienced this before...who can i contact? What i received i love.....',
 'Nope',
 "The hose I received does not recoil as much as I had thought.  The weight of the hose is wonderful.  I can't handle those heavy hoses.  So far, no leaking or holes in the outer casing.  I am very pleased with my purchase.  I will recommend this product to my friends.",
 'Product is holding up well.',
 'Just used it once so far but the product was well made and fit and performed as I expected.',
 'I bought this for my son who hikes and rides trails on his bike. Affordable and durable product.',
 'Have only used it once so far but happy with the foil cutter and how quickly it removed the cork from a bottle',
 'Love them!!  Work awesome',
 'Muy buen libro.',
 'Absolutely love these tins. Perfect for my shave soaps. Very pleased with the quick shipping. Tins came in perfect condition. I would definitely recommend.',
 'I purchased this following hysterectomy surgery as requested by the surgeon.  It is very painful to wear and hard to velcro together.  Most of the swelling has gone down, but will have to wait another week to try again.',
 'I love everything',
 "GOOD PRODUCT, HANDY FOR CARRYING DOG'S WATER AND FOOD.",
 'I ordered it for my hernia but it also helps my back. I just love it.',
 'Love it!',
 'Excelente.',
 "I keep dropping ceramic soap dishes and breaking them in my sink.  With this,the shaving soap is a perfect fit, plus it has a lid so dirt will not enter.It's a tad over priced, but I really like it.",
 "The earings arent sharp and it didn't pierce",
 'The placard holders were perfect!  I ordered another one!  Cupcake holders are also perfect in size.... a bit thin on the plastic,  but they will work.',
 "I like that there are more of them than I thought, but they are smaller than I expected. They will work for what I need though, and that's what is important.",
 'Love them',
 'Looove this! Just what I needed!',
 "It works great, I sweat a lot and it does the job. I would of liked a different color but other than that I'm satisfied with the product",
 'Very durable!',
 'We have parts at work that needs close work and these lights make it a lot easier.',
 'Hi not what i expect aorry i returned',
 'Like it great in smoothies',
 'Love the product',
 'Love these. Great aroma and flavor.',
 'Great little light. Seeing the box for the first time, only about 6" x 2" x 1", I thought I received the wrong light. But upon opening it\'s the correct light. Long cord, more than 3\' without any clumsy transformer and substantial round magnet, ~1" diameter, really holds it in place plus gooseneck makes a great light. Time will tell if it all holds together. No reason to doubt longevity at this point. Features and price can\'t be beat.',
 'I would love it more if there were user directions! How much tea per cup would be SO helpful!',
 'If you consider yourself a steward of common sense, expect to be completely bored by this poorly written and terribly organized collection of words on pages.',
 'So far, so good',
 'Love it!',
 'Tasty!!!',
 'Excelent !!!!',
 'It is mostly common sense stuff, but good for making you think about it all.',
 'love love these',
 'I got this block and not the tablets I expected.  I returned it',
 'Thanx Sarah for helping me with bottle they are the best ever love them will only buy them your great girl',
 "I got a M Size. It fits well around my wrists. However the finger loops are a tight fit. But I'm sure that these will open up after use.Overall they appear to be of good quality material and sturdy build.Will definitely consider another pair when these wear out after use.Thanks Much",
 'Product came quickly and looked great!',
 'OKAY!',
 'ILove it !!!!!!!!',
 'For my grandson love it!😊',
 'Love it',
 'Perfect for what I needed it for!',
 "This is the 4th product I have purchased from Healthy Human.  Tumblers and water bottle steins of different sizes and colors.  The ice stays frozen for close to 4 days-then I just add a hot water bottle I keep in my car and it's instantly cold and perfect!  Customer service is also amazing!",
 '😆👍',
 'They were shipped very quickly and my kids love them!! Great price also',
 'My daughters love them! Fun and they are learning to tell time.',
 'Absolutely love it!',
 "I love it. It's big,beautiful, and sparkly.",
 'I love it',
 'just brilliant',
 'Works well. Well made',
 'Too much um... superfluous text. I wish it would get to the point faster rather than giving you anecdotes.',
 'Love everything I received thank you',
 'Kids love them',
 'Love the "50 & Fabulous" cake topper. Nice quality. Will be a great addition to a friend\'s birthday cake.',
 "This is nicely  compact, so it won't take up much of the work space you are trying to light up. A strong magnet will resist movement. A winner.",
 'very good.thanks.',
 'Love the color! Love the feel! And love the space!',
 'HelloI just wanna say that I absolutely love my healthy human cup. I use it for my green smoothies and my tea, it keep my smoothies cold and my tea hot for hours. I also adore the colors and that fact that I can be discreet and healthy at the same time. Thanks for making such a get product. I am definitely recommending all my family and friends to buy.',
 'Arrived in good shape, looks beautiful will be perfect for my mothers 70th birthday party',
 'Beautiful....',
 'I got it and I like it! Gonna buy another!!',
 'Purchased for out of state grandson and he is loving it. Time will tell how long the velcro closure lasts. He is a tall 2 1/2 year old and the cape is fine in length - the neck is pretty large. He enjoys the mask but edges are stiff and scratchy, making it uncomfortable for extended wear on tender skin.',
 'Love it',
 'Love it!',
 'A must for your shop machines.',
 'Perfect for my vanity space',
 'My son loves his capes. They appear to be handmade and with a lot of love.',
 'Hreat',
 'My grandkids love the superhero capes',
 'Absolutely perfect!',
 'fast delivery, great product.',
 'Fast shipper. Great product. Love it!',
 'My grandson loves the capes! Pleasantly surprised at the quality of the capes. Thank you!',
 'Well made and very functional. This light is easy to control and lights up a cubic meter of space very well.',
 'Craptastic',
 'Perfect for my toddler.  fast turnaround time.',
 'Small and compact.  Magnet holds well to my Wood lathe.',
 'The kids just love them!',
 'Absolutely love it',
 'Just what my dad was looking for to put on his grinder and drill press!! Works great!',
 "Can't wait for our son to open this from Santa!",
 'gives me energy',
 "This is my second !  I'm thinking of buying a third. It pus gather light right where I need it!",
 'Love it',
 'Perfect!!',
 'Amazon added advertising',
 '“Great product”',
 'helps to organize your mind/thoughts and every day execution methods.',
 'After a few hours of writing down everything I have to do (and not finishing) I got too stressed and depressed to list any more. I have gone back to doing things in my own haphazard way. I know some people who swear by this method, but I had to give up before I was swearing AT it.',
 'Great product,nice quality got it delivered really fast',
 'Awesome!!!!!',
 'Love my water bottle!!',
 'Great~',
 'Super cute. Delicate and not flashy. I love it',
 'Nice!',
 'Made a lot of sense.',
 'Very happy with the headband I received from kooshoo. Well made and very comfortable to wear. Will definitely get more.',
 '👍🏼',
 'Beautiful and just want I needed to give to my childhood bestie.',
 'Everything I was expecting! Love the simplicity!',
 'It was not as helpful as I expected.  Not real easy to listen to',
 'Love the gloves.  May be too big if you have small hands.',
 "I love these bibs! They are so soft and pretty absorbant. I don't worry about my little wearing these bibs all day and having a rash around his neck/under his chin. I want all the designs!!",
 'ExcellentVery doable',
 '"Love" my new necklace. I received it quickly and it looks and fits exactly as expected.',
 'Love them all',
 'Very cute and super soft bibs for baby, love them!',
 'Very professional',
 'Excellent. So helpful!',
 'Outstanding!!!!',
 'This product arrived quickly and was exactly what I expected.',
 "Try the bullet journal method if you're looking for a more humanly and less robotic way of organization.",
 'Life-changing.',
 'lj;lk;k',
 'Reeding it',
 "Excellent methodology. I'm starting to get organized and less things in my head!",
 'Fast delivery, great product.',
 'love',
 'Meh.',
 "It's ok!",
 'K',
 'dull',
 'the best methodology I have ever known',
 'How can the Kindle version possibly cost more than the paper version. You have all of the same costs minus printing, shipping, and storage. This is a glaring ripoff.']
```
























-Swapnil
