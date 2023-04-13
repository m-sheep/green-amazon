# Amazon Greenwashing Analysis
This repository contains an analysis of ‘greenness’ in product descriptions of household products and their online marketing.
It was created by Barbora Bromová, Markéta Ovečková, and Šimon Trlifaj for the 2023 CIVICA course Diving in the Digital Public Space.

## Contents
This repository includes:
- Web scraping pipeline of Amazon product pages based on search keyword (Amazon_webscrape.ipynb)
- Analysis of product descriptions and perceived harmfulness (Amazon_greenwashing_analysis.ipynb)
- Analysis of topic modelling in product descriptions (Latent_Dirichlet_Analysis_on_product_descriptions.ipynb)

## Introduction
While the term ‘digital public space’ is usually thought to refer to online spaces specifically designed for social interaction (such as social media sites, forums, chatrooms, or blogs), a significant amount of substantive online interactions takes place in commercial contexts. Perhaps chief among the online marketplaces, Amazon.com is among the Internet’s biggest commercial platforms. Having started as an online bookstore, the company has quickly gained prominence as the quintessential online marketplace and currently carries near-all categories of consumer products from its own inventory as well as third-party sellers. 

Digital spaces dedicated to e-commerce have become not only a focal point of online economic activity, but also of social interaction. As Trevor J Blank points out in his 2015 investigation of Amazon.com product pages, online market places – particularly their interactive components – can become ‘a locus of digital performance’ and foster meaningful online interaction (p. 286). Indeed, many previous research papers have focused on analysing user behaviour in the context of ratings, comments and product reviews, investigating their interaction effects (Park et al, 2021), valence (Kordrostami & Rahmani, 2020), or performativity and subversion of institutional structures (Blank, 2015).

Yet limited literature exists when it comes to systematically analysing the other side of the online marketplace equation: few academic authors consider the sellers, the language of their product descriptions, and the topics of their marketing materials. Some continue to analyse online marketing in terms of customer reviews – for example, Wu & Lin (2017) consider the effects of online brand messaging in terms of their interaction with electronic word of mouth, particularly when it comes to perceived product usefulness. Academic works which discuss the phenomenon of greenwashed marketing on Amazon.com, on the other hand, tend to address the topic more globally and lack an empirical analysis of product pages (see Sands & Morison, 2020). This analysis aims to supplement existing literature by focusing on the communicative practices of commercial sellers on Amazon.com as one of the leading e-commerce platforms, aiming to quantitatively assess how online sellers communicate values particularly when it comes to sustainability discourses.

The topic of sustainability has been gaining prominence in recent years, and the field of e-commerce has seen its fair share of green-washing attempts. In addition to serving as a platform and e-commerce marketplace where sellers present – and thus also greenwash – their goods independently, Amazon as a company has also realised brand value through multiple sustainability commitments, including the The Climate Pledge in 2019 (Sands & Morison, 2020). Yet simultaneously with these sustainability commitments, the company has also relaxed its internal guidelines toward third-party sellers and became recognisably more lenient with cross-border imports (Pinnamaneni, 2018). This shift was also partly reflected in consumers’ experience: despite an apparent increase in the supply and diversity of goods, there seemed to be a shift in discourse and marketing conventions on the platform (Pinnamaneni, 2018). Over time, the names and descriptions of products became longer and less intuitive, serving as catch-all for relevant keywords and aiding search engine optimisation. Yet could these new naming practices be informed by new values, such as sustainability? This analysis intends to investigate the discourse present within product descriptions on Amazon.com, focusing particularly on their attention to eco-friendliness.

## Research question & Hypotheses
As sustainability concerns gain a greater visibility in public discourse, there appears to be increased pressure on industry to follow suit and minimise the adverse ecological impact of their products (Sands & Morison, 2020). The field of e-commerce is no exception in this context. In addition to serving as a platform and e-commerce marketplace where sellers present (and thus also ‘greenwash’) their goods independently, Amazon as a company has also realised brand value through multiple sustainability commitments, including the The Climate Pledge in 2019 (Sands & Morison, 2020).

While there is an ongoing debate about the practical impact of sustainability commitments on the design and manufacturing of products, the trend of ‘greenwashing’ in product marketing has been well documented as a common response to calls for more sustainability (Sands & Morison, 2020). Can this trend be tracked on digital marketplaces? This research report aims to investigate the extent to which Amazon suppliers attempt to compensate for a priori non-eco-friendly products association in product descriptions. It aims to test three distinct hypotheses:

> H1: There is an identifiable ecological discourse in product descriptions on Amazon.com.

> H2: The frequency of ecological words and discourses varies across product categories marketed on Amazon.com.

> H3: Sellers are more likely to compensate for the non-eco-friendly perception of products with a higher occurrence of ‘ecological’ keywords, thus engaging in ‘greenwashing’.

## Data
While Amazon.com remains a widely accessible online marketplace, gaining access to its data for analysis is not trivial. Amazon does offer automated data access through a dedicated API, yet this application is not intended for research and exists primarily as a service for Amazon third-party sellers. The data used for this analysis were therefore web-scraped using the Requests library version 2.28.0; the relevant code is available in the ‘Amazon_webscrape.ipynb’ notebook. Scraping results were saved in a compressed file named products.csv.gz, which contained 4,014 product pages over 71 pre-defined categories of household products. Each product page observation included the relevant search keyword, product name, product description, brand name, product page url, and webpage. 

## Method
### Unsupervised topic modelling using Latent Dirichlet Allocation
To identify the topics present throughout scraped product descriptions, this analysis used latent dirichlet analysis (‘LDA’). LDA is an unsupervised topic modelling method commonly used for identifying and exploring latent topics across smaller text corpuses, classifying tokens and thus grouping textual data into groups according to patterns which often fall below the threshold of human recognition. This method – and its associated Python libraries, such as pyLDAvis – ensures that documents can be assigned to topics, topics themselves can be identified, revised, and visualised in relation to each other. In addition, this method also allows for new synthetic documents to be created to precisely reflect the topic distribution present in the original training corpus. 

The notebook Latent_Dirichlet_Analysis_on_product_descriptions.ipynb includes codes used in this method.

### ‘Green’ compensation of non-ecological word embeddings
To measure the use of green adjectives to compensated for perceived harmfulness of products, this analysis developed two metrics based on word embeddings: a measure of perceived harmfulness to the environment, and a predefined corpus of greenwashing vocabulary to account for compensatory occurrence. 

First, the Wikipedia-trained language model glove-wiki-gigaword-100 was used to create a measurement of perceived harmfulness of product categories. The main axis was created by taking the mean vector of a group of words with negative ecological connotations (such as pollution, chemical, deforestation, etc.), and another with positive connotations (sustainable, ecology, nature, etc.). The product categories were then given a score depending on how close they would be to one or the other, and ranked accordingly. Second, this analysis counted the average frequency of adjectives that tend to appear when emphasising the environmentally friendly (or ‘green’) properties of a product (such as eco, bio, green, renewable, etc.). This frequency was then correlated with the perceived negative ecological connotation and the use of green adjectives.

The notebook Amazon_greenwashing_analysis.ipynb includes codes used in this method.

## Analysis
### H1: Unsupervised topic modelling does not capture an explicit ecological discourse
In addressing the first research question, a latent dirichlet allocation model could not identify a distinctly environmental topic among our dataset’s documents. When optimising the model to maximise topic coherence, we added additional stopwords to the standard set for English language datasets, going off an initial exploratory analysis of word frequency in product descriptions.

![wordcloud on  'product_description'](https://user-images.githubusercontent.com/115785307/231808831-35550f60-4525-4749-8b24-f403dfcd388d.png)

The final configuration of our model identified 7 topics, with α = 0.31 and β = 0.91. These parameters increased the model’s initial coherence score of 34.3% by almost double, to a final coherence score of 59.04%. To explore the topic inter-relations, we generated a visualisation of relative inter-topic distance.

![improved model](https://user-images.githubusercontent.com/115785307/231810267-4de924a8-b3e2-4f6c-9bee-7a4a794a4cd2.png)

While the model seemed to identify three main topic clusters, it did not pick up on a topic that could be categorised as specifica ecological. In this sense, unsupervised topic modelling could not support our hypothesis that there exists a (machine-) identifiable environmental or ‘greenwashing’ discourse in product descriptions on Amazon.com. However, it should be noted that the model did seem to pick up on a measure of naturalness or healthiness in the topic clusters, sorting them left to right from the most healthy and clean (eg. the most relevant terms of Topic 1 included ‘free’, ‘clean’, ‘safe’ and ‘water’), to artificial and harmful (eg. the most relevant terms for Topic 7 included ‘bacteria’, ‘disinfectant’, ‘germs’ and ‘viruses’). 

The html version of this LDA visualisation can be found in Latent_Dirichlet_Allocation_HTML.html.

### H2: There is some variation in the prevalence of ecological words across product categories
To address the second hypothesis, we used a pre-defined corpus of greenwashing vocabulary to count the occurrence of these tokens, and plotted their prevalence across distinct product categories. Indeed, we found that the average occurrence of ecological and sustainability-related words differed across product categories and search keywords. Comparing the relative occurrence, greenwashing vocabulary seemed to be most prevalent with laundry products, and least prevalent among products intended for disinfecting surfaces. 

![greenwashing visualisation](https://user-images.githubusercontent.com/115785307/231810648-8ac0dde7-f7bb-493b-8b20-0f90b3d902ca.png)

### H3: No evidence for systematic compensation for ecologically harmful word embeddings
The third and final section of our analysis set out to investigate whether a more frequent use of greenwashing vocabulary could be in response to the non-environmental associations a product category inspires, as anticipated by the third hypothesis. First, we sorted the dataset’s search keywords onto a word vector thought to symbolise their extent of implied environmental harm, and plotted the average frequency of greenwashing words in their product descriptions.

![compensation graph](https://user-images.githubusercontent.com/115785307/231811023-7f074acf-5b76-4f99-acb4-cb67ff654e4f.png)

Our third hypothesis anticipated a (somewhat) linear positive trend between a product category’s perceived environmental harm gleaned from word embeddings and its prevalence of greenwashing terms. We did not find such a trend. The outliers we found – such as laundry products, which showed the highest frequency of greenwashing terms – were relatively neutral in terms of their word embeddings. 

The notebook Amazon_greenwashing_analysis.ipynb includes codes used for this analysis.

## Conclusion
This analysis found that even though ‘green’ words did feature in product descriptions and their prevalence varies across product search terms, there was no systematically identifiable greenwashing discourse – at least not one that is consistent across product categories. To this extent, we were not able to support our initial hypotheses, with the exception of Hypothesis 2: in the dataset, there was some evidence that product categories vary when it comes to their prevalence of greenwashing discourses. The pattern we anticipated – a systematic compensation for environmentally-unfriendly word embeddings – was not demonstrated in this analysis.

There may be multiple reasons for this, as this analysis is rather limited in its scope. It may be that sellers do not compensate for the perceived harmfulness of a product in the hypothesised way. Alternatively, it may be possible that this analysis included too diverse of a product range or that a greenwashing compensation occurs with visual cues rather than within textual product descriptions. Future research endeavours could explore these elements of online product marketing and sustainability further, particularly when it comes to visual expression and packaging representation of products. 

## References
Blank, T. J. (2015). Faux Your Entertainment: Amazon.com Product Reviews as a Locus of Digital Performance. Journal of American Folklore 128 (509), pp. 286–297. 

Kordrostami, E. & Rahmani, V. (2020). Investigating conflicting online review information: Evidence from Amazon.com. Journal of Retailing and Consumer Services 55 (2020) 102125. 

Park et al (2021), “Worse Than What I Read?” The External Effect of Review Ratings on the Online Review Generation Process: An Empirical Analysis of Multiple Product Categories Using Amazon.com Review Data. Sustainability (13) 10912. 

Pinnamaneni, S. (2018). #124 The Magic Store. Reply All. Available at: https://gimletmedia.com/shows/reply-all/brhow4.

Sands, H. & Morison, B. (2020). Greenwashing in the Information Industry. The iJournal 5 (2), DOI: 10.33137/ijournal.v5i2.34413. 

Wu, T. & Lin, C. A. (2017). Predicting the effects of eWOM and online brand messaging: Source trust, bandwagon effect and innovation adoption factors. Telematics and Informatics 34 pp. 470–480.
