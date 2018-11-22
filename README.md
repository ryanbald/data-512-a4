# A4: Final Project Plan

## Background

The King County health department implemented a [new food safety rating system](https://www.kingcounty.gov/depts/health/environmental-health/food-safety/inspection-system/food-safety-rating.aspx) beginning in 2017, providing the public with more visibility into how well a food establishment is performing on their mandated food safety inspections. Under this new system, food establishments have been required to post a sign on their windows displaying the result of their most recent inspection: either "excellent", "good", "okay", or "needs improvement". One of the goals of the new food safety rating system was to help the public "make informed decisions about eating out", giving the results of food safety inspections a direct influence on how well a food establishment business performs. With how much inspection results affect businesses, it's paramount that the food safety code being used to judge these food establishments includes no biases.

## Research Question/Hypothesis

I will be performing an analysis on food establishments in King County and their health department inspection results. I hope the analysis will act as a sort of audit on the food safety code that food establishments in King County are being judged against. The main question I will be asking is:

> Is King County's criteria for judging an establishment's food safety practices inherently biased against food establishments that serve certain non-American cuisines?

With my hypothesis being:

> Food establishments that serve certain non-American cuisines typically receive lower inspection scores than their counterparts who serve American cuisine because there are specific violations that occur much more frequently while inspecting food establishments that serve certain non-American cuisines.

With the reason behind my hypothesis being that these specific violations occur much more frequently because of the methods necessary to prepare foods from certain non-American cuisines and not because of actual differences in food safety considerations. To further investigate the reasoning behind my hypothesis, I may need to reach out to experts or people with more knowledge than me about what is needed for certain non-American cuisines to be prepared.

## Data Sources

### King County Food Establishment Inspection Data

King County provides an [open, downloadable, frequently updated dataset](https://data.kingcounty.gov/Health-Wellness/Food-Establishment-Inspection-Data/f29f-zza5) consisting of food estalishment inspection results dating back to January of 2006. The inspection data is released under a [public domain license](https://wiki.creativecommons.org/wiki/Public_domain), meaning "it is free to use by anyone for any purpose without restriction under copyright law". Each row in the dataset represents an inspection where for inspections with multiple violations, each violation will be on its own row. I suspect I'll need to process the dataset so each row truly represents an inspection and multiple violations during a single inspection can be represented on a single row. The schema for the food inspection dataset consists of 22 columns; here are the columns of note:

|Name(s)|Description|
|-------|-----------|
|Name/Program Identifier/Inspection Business Name|all 3 of these columns closely represent what can be perceived as the food establishment's name|
|Inspection Date|date of health department food safety inspection|
|Address/City/Zip Code|together, these 3 columns represent the full street address location of the food establishment|
|Phone|food establishment's phone number|
|Longitude/Latitude|together, these 2 columns form the geographical coordinates location of the food establishment|
|Inspection Score|the score result of the health department inspection, higher scores mean worse results|
|Violation Description|description of the listed violation, which looks like it comes directly from the [King County food establishment inspection report form](https://www.kingcounty.gov/depts/health/environmental-health/food-safety/inspection-system/~/media/depts/health/environmental-health/documents/food-safety/sample-food-inspection-form.ashx)
|Violation Points|the severity of the listed violation, more points mean higher severity|

### Yelp Fusion Business Endpoint Search APIs

[Yelp provides multiple search APIs](https://www.yelp.com/developers/documentation/v3) that I will be utilizing primarily to retrieve the cuisine type of as many food establishments with inspections from the King County health department as possible. [Yelp's APIs terms of use](https://www.yelp.com/developers/api_terms) is mostly strict, but provides exceptions for non-commercial use of their APIs. The terms of use states that "you may use the Yelp Content to perform certain analysis for non-commercial uses only, such as creating rich visualizations or exploring trends and correlations over time, so long as the underlying Yelp Content is only displayed in the aggregate as an analytical output, and not individually". To abide by their terms of use, I will make sure to not display individual results from the search APIs and only display aggregate measurements based on cuisine types.

Yelp provides a [phone search API](https://www.yelp.com/developers/documentation/v3/business_search_phone) that allows you to search for a restaurant using a restaurant's phone number and also provides a [more general search API](https://www.yelp.com/developers/documentation/v3/business_search) that allows you to search for a restaurant using the restaurant's location, either full street address or geographical coordinates, and lets you optionally provide a keyword as well. Both search APIs return any restaurants that match the search parameters in a JSON format, providing many fields relating to the Yelp restaurants, but the only field from the restaurant objects that I will be interested in are the categories/cuisines the restaurants are tagged as.

### Merging

I will be able to use the food establishment phone numbers from the King County inspection dataset to match food establishments with Yelp restaurants through their phone search API. I will also be able to use both street address locations and geographical coordinate locations from the inspection dataset to match food establishments with Yelp restaurants through their general search API, with the additional option to use any representation of the food establishment's name from the inspection dataset as a keyword to improve the Yelp search. Using this redundancy, through numerous Yelp API calls, I suspect I'll be able to densely populate an additional field in the King County food establishment inspection dataset representing the cuisine type of the food establishment being inspected.
