##### Week 07 Contents
- Presentation: [Census](readme.md)
- Code: [Working with the Census API](census-api.md)
- Code: [Python Census Data Plotting](census-plotting.md)
- Homework: TBD

-----

### First Steps

Please request a [Census API Key](https://api.census.gov/data/key_signup.html).

Also, install Pandas and Geopandas on your computers (and later Raspberry Pis).

```
pip install pandas
pip install geopandas
```

-----

###### What is the Census?

The [US Census](https://en.wikipedia.org/wiki/United_States_Census) — an extraordinary resource for data scientists, policymakers, and social science researchers — is an attempt to collect information on every single person living within the boundaries of sovereign United States territory every ten years. It is called for by the Constitution of the United States, and has never been missed (even taking place during the United States Civil War ). Not only is the census vital for a better understanding of the makeup and condition of the United States, but it also serves as the official mechanism for drawing congressional and state goverment district boundaries — rendering the census a vital component of the United States political mechanism. Thousands of taxpayer-supported census workers campus the country, knock on doors, collect data, and ensure as accurate a count as possible through some [functionally questionable instruments](https://www.census.gov/2010census/pdf/2010_Questionnaire_Info.pdf).

Though an effort is made to deliver a wholistic and objective count with financial punishment for those who do not complete the census form ($100/household in 2010) and a promise of total confidentiality and indemnity, participation in the census is estimated at 74%, with some populations statistically underreporting. For instance, undocumented immigrant families are less likely to participate for fear of governmental reprisal. This skew has been modeled, and serves as a statistical adjustment in some of the census datasets which do not report raw numbers. Most census datasets, however, report nothing more than pure integer counts, and contextual offsets are left up to the data modeler. 

Modeled on an Ancient Roman equivalent that tracked citizenry for military draft purposes (*censore* in Latin and its derivative in most romance languages means *to determine value of something*), the success of the US census has served as a model for similar efforts across the globe and supported by the United Nations. Many countries complete their own censuses, and provide them to citizens in a similar way to the United States model. 

-----


###### Criticisms and Limitations of the Census

The Census is decribed in the US constitution.

> Representatives and direct Taxes shall be apportioned among the several States which may be included within this Union, according to their respective Numbers, which shall be determined by adding to the whole Number of free Persons, including those bound to Service for a Term of Years, and excluding Indians not taxed, three fifths of all other Persons. The actual Enumeration shall be made within three Years after the first Meeting of the Congress of the United States, and within every subsequent Term of ten Years, in such Manner as they shall by Law direct. The Number of Representatives shall not exceed one for every thirty Thousand, but each State shall have at Least one Representative; and until such enumeration shall be made, the State of New Hampshire shall be entitled to chuse three, Massachusetts eight, Rhode-Island and Providence Plantations one, Connecticut five, New-York six, New Jersey four, Pennsylvania eight, Delaware one, Maryland six, Virginia ten, North Carolina five, South Carolina five, and Georgia three.

As is obvious from this Article 2 US Constitutional mandate, the incredible resource that is the census is definitively tarnished by historical realities. The censuses of the early United States did not count women, people of color, or indigineous peoples equitably, until the landmark census of 1850 — which was administered by the ahead-of-his-time sociopoltical activist and Civil War cartographer and strategist [Joseph C.G. Kennedy](https://en.wikipedia.org/wiki/Joseph_C._G._Kennedy). There were regressions later, however — for example the census of 1940 was used surreptitiously to identify potential Italian, German, and Japanese sympathizers for illegal incarceration. Because the census strives to offer consistent data over time, it often uses non-contemporary terminology and selection biases. For instance, the census still maintains binary gender definitions, counts same-sex marriages in some states and not others, and mixes up racial and ethic identity, Counting people, inherently, requires the drawing of lines between people — and that is often done by the census in unenlighted, conservative ways — with consistency as the ultimate goal.

Moreover, because of the role of the census in determining House of Representative seat counts, census data collection is often politicized. The 2020 census is already being accused of potential misuse mostly surrounding the counting of undocumented immigrants, and despite its importance, is under threat of defunding by both political sides. Additionally, contemporary gross population trends are much more dynamic than the every 10 year original model designed for the on-horseback census worker. As a result, contemporary census data collection efforts have expanded.

-----

###### American Community Survey
Since the early aughts, the 10 year cadence of the census — clearly an outdated model in an era of constant data production and collection — has been supported by a motley assortment of supplemental data gathering efforts. One of the more successful attempts has been the American Community Survey, which aims to provide census-level details of a statistical sampling of the full American population at a higher frequency for large population centers. The ACS occurs on an ongoing basis through the calendar year with a (relatively) small sample size (population centers of 65,000 individuals or greater), and is also tallied every five years with a larger sampling (population centers of 20,000 individuals or greater).

The ACS tracks around 60,000 *variables*, and provides projections and estimates for another 20,000 or so. The data is voluminous, to say the least. Many of these indicators are aimed at constructing a picture of the United States' identity makeup — gender, race, ethnicity, derivation, age. Others are financial — income, employment status, employment industry, labor force participation. Further indicators support a better understanding of the American household — children/family, household size, number of rooms, rent/mortgage cost, frequency of housing changes, languages spoken at home. A large proportion of these variables have been tracked in the same way over many years, allowing data scientists and visualizers to trace and understand the increasingly tumultuous social dynamics of the United States.