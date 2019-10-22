##### Week 07 Contents
- Presentation: [Census](readme.md)
- Code: [Working with the Census API](census-api.md)
- Code: [Python Census Data Plotting](census-plotting.md)

----------

###### Census API

The Census and ACS are available online, along with an increasing number of historical census datasets and other data repositories, as a structured API.

Through the use of the Census API, very specific queries can be generated to access information at different geographic resolutions — from whole country tabulations to data bucketed for each state to county- and block-level raw counts.

The [Census API homepage](https://census.gov/developers/) offers a good sense of what is avaiable, though highly specific language is often used.

The [available APIs](https://census.gov/data/developers/data-sets.html) are also enumerated and you can find the ACS there, and [many exploratory tools](https://census.gov/data/data-tools.html) are provided.

-----

###### API Key

The first step to accessing most APIs is to acquire an API cryptographic key.

	> A cryptographic key is a long, alphanumeric, random string that uniquely identifies each user.

Despite census data being fully public, API access is gated to prevent abuse and to anonymously track developers who access the repository. This tracking is benevolent, and helps the maintainers of the API know what is interesting to developers, and where their queries fail unexpectedly.

[Request an API Key](https://api.census.gov/data/key_signup.html), which will be sent to the email address you provide within 15 minutes or so. It may go to your spam folder.

Keep this 40 character string to yourself, and never share it online. Doing so could allow someone else to masquerade as you and hammer the Census API servers. Make sure you copy the string somewhere so that it will be easily accessible. Each API Key is limited to 500 requests per day.

-----

###### Browsing the API Data

The [5 year ACS API](https://census.gov/data/developers/data-sets/acs-5year.html) offers many different way of learning about the data available within. The page lays out four main, confusingly named presentations of the underlying data.

	> Detail Tables : Actual raw datapoints, mostly pure counts, the most data at the highest specificity
	> Subject Tables : The raw data points scaled up and converted into population percentages and averages
	> Data Profile : Percentages and estimations of larger statistical trends derived from the data points
	> Comparison Profile: Shows changes over time for specific data points

The querying format is different for each type of presentation, though the process is similar. 

Underneath each presentation is an set of resources that looks something like this:

-----

###### Detail Tables
- Example Call: api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E&for=state:*&key=...
- 2017 ACS Detail Table Variables [ html | xml | json ]
- ACS Technical Documentation
- Examples and Supported Geography

Selecting the [html](https://api.census.gov/data/2017/acs/acs5/variables.html) link will display a webpage listing all of the data dimensions available and is the best place to start browsing. On that linked page thousands of rows will display exactly what is avaiable in the ACS.

| Name        | Label           | Concept  | Required | Attributes | Limit | Predicate Type | Group | Valid Value |
| :------     |:-------------   | :-----   | :------- |:---------  | :-----| :------------- |:------| :-----------|
|   C15010_001E   |   Estimate!!Total   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER   |   not required   |   C15010_001M, C15010_001EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_002E   |   Estimate!!Total!!Science and Engineering   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_002M, C15010_002EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_003E   |   Estimate!!Total!!Science and Engineering Related Fields   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_003M, C15010_003EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_004E   |   Estimate!!Total!!Business   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_004M, C15010_004EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_005E   |   Estimate!!Total!!Education   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_005M, C15010_005EA   |   0   |   int   |   C15010   |   N/A   |
|   C15010_006E   |   Estimate!!Total!!Arts, Humanities and Other   |   FIELD OF BACHELOR'S DEGREE FOR FIRST MAJOR FOR THE POPULATION 25 YEARS AND OVER	not required   |   C15010_006M, C15010_006EA   |   0   |   int   |   C15010   |   N/A   |


All of the columns hold important information.

- Name : The unique identifier for each data set. This is necessary for calling an API.
- Label : The unique, human readable name for each data set. Most ACS fields will display 'Estimate' since not everyone is being counted.
- Concept : The overall idea of what is being tracked within the dataset, usually understandable.
- Required : If some specific variable was required in order to make a query resolive, it would be marked as such here.
- Attributes : The name + 'M' is an accounting for the margin of error data point for the data set. The name + "EA" is any statement available about how the estimate or average was calculated.
- Limit : If there was a limit on the number of respondents. 0 means 'no limit.'
- Predicate Type : What kind of data is in the data set. Most ACS fields are 'int,' integers, or 'strings,' textual descriptions. 
- Group : Most data sets belong to a group of related datasets.
- Valid Value : Some data sets only permit certain responses. Links here describe those options.

By selecting the [Examples and Supported Geographies](https://api.census.gov/data/2017/acs/acs5.html) link it is possible to find many examples of what you can query the API for, and how. These are great resources! 

-----

###### Calling an API

Let's compose a simple query of the ACS.

For instance, we could attempt to find counts of those who live in the State of Illinois who graduated with STEM Bachelor degrees in 2017. Since we are interested in counts alone, the *Detail Tables* presentation is most appropriate. The "Example Call" descibes how to construct an API call, which looks like a standard web address.

```
api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E&for=state:*&key=...
```

To complete our call, we need some specific pieces of information.

- Name of the dataset, found above. Many can be called together, separated by commas. 
- Geographic domain of interest. Many can be called together, separated by commas. FIPS encoded.
- ACS year of interest
- ACS collection time period (1, 3, or 5)
- API key

```
api.census.gov/data/ |YEAR| /acs/acs |TIME PERIOD| ?get= |NAME| &for= |GEOGRAPHY| &key= |KEY|
```

We already have many of the necessary pieces. The unique name of the data set is `C15010_003E` for the year `2017`, and we've requested an API key previously. The only thing missing is the geographic area of interest.

-----

###### Encoded Geographies

Thousands of geographic areas are encoded with the [FIPS encoding system](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) and accessible for querying. Every state, county, metropolitan area, congressional district, and census tract has a unique numerical id, which can be [found here](https://www.census.gov/geo/reference/codes/cou.html). A list of [state FIPS codes](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code) is also avaiable for easy browsing. The state of Illinois is `17`.

So, our call would look like this, make sure your substitute your API key instead!

```
api.census.gov/data/2017/acs/acs5?get=C15010_003E&for=state:17&key=...
```

Entering that into any web browser will return the following text.

```
[["C15010_003E","state"],
["283557","17"]]
```

The API confirms that `C15010_003E` was accessed at the `state` level. `283557` individuals have STEM related degrees in FIPS state `17`, Illinois.

We could similarly ask for all states by using the wildcard operator `*`. Again, ensure you substitute your API key.

```
api.census.gov/data/2017/acs/acs5?get=C15010_003E&for=state:*&key=...
```

```
[["C15010_003E","state"],
["84646","01"],
["14711","02"],
["139304","04"],
["50278","05"],
["692652","06"],
["116557","08"],
["76857","09"],
["20035","10"],
["11014","11"],
["432861","12"],
["190302","13"],
["32736","15"],
["33691","16"],
["283557","17"],
["134891","18"],
["59116","19"],
["62926","20"],
["81884","21"],
["90705","22"],
["31382","23"],
["141516","24"],
["168557","25"],
["205751","26"],
["128965","27"],
["50784","28"],
["115480","29"],
["26978","30"],
["45956","31"],
["49666","32"],
["30866","33"],
["204546","34"],
["35635","35"],
["436594","36"],
["205115","37"],
["19826","38"],
["247831","39"],
["69776","40"],
["81895","41"],
["292374","42"],
["24036","44"],
["81485","45"],
["21036","46"],
["129196","47"],
["457374","48"],
["58726","49"],
["12151","50"],
["171380","51"],
["152394","53"],
["34796","54"],
["133290","55"],
["13783","56"],
["60478","72"]]
```

Note that there 52 data points. The District of Columbia and Puerto Rico are included.

-----

#### Complex Variable Queries

We can, as mentioned previously, make a query for many variables at a time — indexed by the same geographic indicator. All named variables are separated by commas, with a total limit on 50 simultaneous requests. The variable "NAME" will include the name of the geographic entity in each entry, which is very useful for disambiguation.

```
api.census.gov/data/2017/acs/acs5?get=NAME,B01001_003E,B01001_027E&for=state:*&key=...
```

Note how, in the structure of the response, all of the values assosicated with each geographic entity are included within a singular array. This makes data comparison and visualization significantly simpler.

```
[["NAME","B01001_003E","B01001_027E","state"],
["Puerto Rico","88669","83530","72"],
["Alabama","149143","143020","01"],
["Alaska","27737","26530","02"],
["Arizona","222144","212613","04"],
["Arkansas","97807","92337","05"],
["California","1275266","1218279","06"],
["Colorado","170797","163453","08"],
["Connecticut","95198","90990","09"],
["District of Columbia","22117","21490","11"],
["Delaware","28103","27179","10"],
["Florida","564540","540822","12"],
["Georgia","334296","323132","13"],
["Hawaii","46903","44514","15"],
["Idaho","57926","56186","16"],
["Illinois","401526","384034","17"],
["Indiana","214334","203995","18"],
["Iowa","100258","96227","19"],
["Kansas","100580","96246","20"],
["Kentucky","141397","132743","21"],
["Maine","33005","31389","23"],
["Louisiana","157993","152438","22"],
["Massachusetts","185469","177386","25"],
["Maryland","187106","179644","24"],
["Michigan","293038","278961","26"],
["Minnesota","179100","170862","27"],
["Mississippi","96986","93869","28"],
["Missouri","191383","181758","29"],
["Montana","31504","29714","30"],
["Nebraska","67210","63617","31"],
["New Hampshire","32863","31370","33"],
["Nevada","92869","88338","32"],
["New Jersey","268912","257804","34"],
["New Mexico","66963","64099","35"],
["New York","602196","574681","36"],
["North Carolina","307930","296053","37"],
["North Dakota","26236","25530","38"],
["Ohio","356198","339506","39"],
["Oklahoma","135538","129575","40"],
["Oregon","118838","113016","41"],
["Pennsylvania","364406","347241","42"],
["Rhode Island","27882","26689","44"],
["South Carolina","147510","142454","45"],
["South Dakota","30939","29273","46"],
["Tennessee","206035","196543","47"],
["Texas","1013569","968281","48"],
["Utah","130465","123549","49"],
["Vermont","15500","14329","50"],
["Virginia","261109","248813","51"],
["West Virginia","51806","49239","54"],
["Washington","229189","218956","53"],
["Wyoming","19174","18283","56"],
["Wisconsin","172829","164643","55"]]
```

-----

#### Complex Geographic Queries

The same logic applies to multiple geographic entitities. For instance, we can ask for specific states like Illinois, Wisconsin, and Michigan — all separated by commas.

```
https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_003E,B01001_027E&for=state:17,55,26&key=...```
```

This will return a subset of the result above.

```
[["NAME","B01001_003E","B01001_027E","state"],
["Illinois","401526","384034","17"],
["Michigan","293038","278961","26"],
["Wisconsin","172829","164643","55"]]
```

We also can use the `in` keyword to include smaller geographic entities constrained by larger ones. For example, perhaps we could ask the API for data for all of the congressional districts in Illinois. Note that `congressional district` has its space replaced by `%20`, which is the HTML encoding for a space. All spaces found in supported geographies will be similarly replaced.

```
https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_003E,B01001_027E&for=congressional%20district:*&in=state:17&key=...
```

The results will include all 18 congressional districts that Illinois currently holds. Illinois will likely lose a congressional seat as a result of the 2020 census, so be careful when comparing this kind of flexible geographic determiner over time.

```
[["NAME","B01001_003E","B01001_027E","state","congressional district"],
["Congressional District 3 (115th Congress), Illinois","24147","22880","17","03"],
["Congressional District 1 (115th Congress), Illinois","20753","21578","17","01"],
["Congressional District 2 (115th Congress), Illinois","21766","20820","17","02"],
["Congressional District 4 (115th Congress), Illinois","26829","24276","17","04"],
["Congressional District 5 (115th Congress), Illinois","24056","21926","17","05"],
["Congressional District 6 (115th Congress), Illinois","20952","20121","17","06"],
["Congressional District 7 (115th Congress), Illinois","23810","22589","17","07"],
["Congressional District 8 (115th Congress), Illinois","24678","23495","17","08"],
["Congressional District 9 (115th Congress), Illinois","19860","20652","17","09"],
["Congressional District 10 (115th Congress), Illinois","23046","22004","17","10"],
["Congressional District 11 (115th Congress), Illinois","23721","23407","17","11"],
["Congressional District 12 (115th Congress), Illinois","21996","20434","17","12"],
["Congressional District 13 (115th Congress), Illinois","20180","18959","17","13"],
["Congressional District 14 (115th Congress), Illinois","21903","20573","17","14"],
["Congressional District 15 (115th Congress), Illinois","20733","20274","17","15"],
["Congressional District 16 (115th Congress), Illinois","20020","18684","17","16"],
["Congressional District 17 (115th Congress), Illinois","22045","21351","17","17"],
["Congressional District 18 (115th Congress), Illinois","21031","20011","17","18"]]
```

Be sure to explore the many other examples available in the 'Examples and Supported Geographies' links found under each data presentation on the [ACS page](https://census.gov/data/developers/data-sets/acs-5year.html).

Also, note that all geographies are *FIPS encoded*. Here are some resources to map names to FIPS codes.
- [States and Regions](state-regions-fips.xls)
- [Counties](counties-fips.xls)
- [Metropolitan, Micropolitan, and Combined Statistical Areas](metropolitan-csa-fips.xls)

-----

###### Saving Results and Data Structure

The arrays that result from a census query can be copied and pasted elsehwere from your browser, or alternatively, saved into a file. By default, your browser will likely append the `.json` extension, short for 'Javascript Object Notation.' The census uses a nonstandard form of JSON in order to be compatible with spreadsheet programs like Microsoft Excel, Google Sheets, or Apple Numbers. Once you have downloaded the file, changing the extension to `.csv`, 'Comma-Separated Values,' will allow spreadsheet programs to read and manipulate the queried data in a more familiar context. You may want to remove all "[" and "]" characters so that Excel, Numbers, and Sheets do not read the rows incorrectly.

As you spend time exploring data without the crutch of spreadsheet programs, your ability to identify structures and find flaws and inconsistencies within datasets will quickly improve and ease your modeling work down the line.

As has been visible throughout the queries above, the data that is returned from the census takes on the form of a 2-dimensional matrix.

```
[
	["value","geoID"],
	["value","geoID"],
	["value","geoID"]
]
```

A query returns a single *parent* array (the top and bottom square brackets). Within that array, each geographic *child* entity is its own array in the matrix, and each requested data point is contained in that array. Often, towards computational modeling goals, this data model is too simplistic — and the work of the data modeler often is to match, manipulate, and reconstruct the data in order to serve the intended presentations. This is another reason to avoid using spreadsheet programs, as these software tools only permit 2-dimensional matrices and not the often complex, nested, and multidimensionally hierarchical data structures enabled by a *plain text* approach to data.

-----

###### Looking Forward and Review

You can find more information and details at the many links above, and in [this guide](https://www.census.gov/content/dam/Census/data/developers/api-user-guide/api-guide.pdf) produced by the census development team — it's a highly recommended read.

A base census api call for Chicago census tracts might look like this...

```
https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E&for=tract:*&in=county:031&in=state:17&key=...
```
