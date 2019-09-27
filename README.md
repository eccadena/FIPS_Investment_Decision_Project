# FIPS Investment Decision Project
Usage of US Census Bureau FIPS (Federal Information Processing Standards, 11-Digit Code, concatenation of state, county, tract code) and area data (including, but not limited to Food Desert data, Eviction Rates, Social Vulnerability index) to make better informed decisions for investing (Real Estate, specifically) in a specific area (in the Houston area, specifically).

# Project Specifications

### Test Website (“RentRiskIndex”): 
http://rentrisk.info/

### Google Slides, a summary presentation of the material: 
https://drive.google.com/file/d/1t95wObq8hKZCwmkIaKG69lVkdKHJhuuJ/view?usp=sharing

### Photos used for presentation are saved in: 
reports > images.

# Driving Questions of this project:

### Where to find interesting and free data pertaining to the ‘Why?’ of our project?

The ‘why?’ that drove our project was simple: Help people make informed decisions about where they live or invest. Our first thoughts were geared primarily to real estate and structuring our data and analysis around how much money could be had given X number of factors. Our focus began to switch when useful and meaningful data on real estate became increasingly difficult to find for free. Luckily it was noted that many agencies, bureaus and academic entities used FIPS to record attributes and characteristics and placed this data out for use. From there it was simple… how many meaningful attributes could we find as it related to an area’s FIPS? 

Lucky there was plenty to choose from and much of which was free. We found ourselves in a surplus of attributes for any given FIPS in the US. This lead us to both shorten our list of attributes and shrink our area of interest to just the Harris County/Houston Area. We focused on attributes such as an area’s Food Desert Rating, compiled by the Department of Agriculture, eviction rates and poverty rates, compiled by Princeton University and Social Vulnerability, compiled by the Agency for Toxic Substances and Disease Registry, to name a few.

### Are the attributes in our dataset (join_data) correlated?

To answer this question, we used pandas .corr() to analyze how our attributes were correlated with one another, and whether positively or negatively, in Harris County. This data frame of values was insufficient in the efforts to gain real insight into how the attributes were relating. Pandas’ heatmap was pivotal in pinpointing the attributes with the greatest correlations with ease through a simple color scale of blue to red (strong positive to strong negative correlations, respectively). 
A further analysis was given to these attributes using interact (from the panel library) such that a scatterplot (from hvplot/pyviz) is rendered immediately upon call. This, along with plotting each FIPS point by size of its population, allowed us to see what behaviors are possibly occurring between attributes. However, the same issue of insufficiency arose but now with not fully gaining prospective of where this is happening in the greater Harris County area and how, although outside the scope of this project, causation is playing a role (an issue resolved using Folium; read below for solution).

### Where in Harris County were these trends happening the most or the least?

Python has a library similar to that of Pandas called GeoPandas which stores information by columns while also including a column for geographical information, such as how the area is shaped on a map, how big it is and latitude and longitude. This data frame, by itself, was not useful. However, GeoPandas’ .plot() allowed for a very easy rendering of a map with the geological properties of the dataframe being mapped. Although, much like the heat map and scatterplot renditions, this was suitable to viewing and areas attributes via a color scale. However, there was lack of functionality in the form of not having hover events, scrolling capabilities and map details like that you can see from a traditional map or even Google Maps.
The library Folium allowed for easy integration of our dataframe of attributes onto a Choropleth. This choropleth does what GeoPandas did but with all the functionalities GeoPandas was missing. This allowed for the user to not only see where their FIPS were but to also see how vastly different each of the FIPS was from each other when viewing only, say, School Ratings or even plotting multiple attributes such as Poverty and Food Desert information. 
However, this information now had a generic-flavor that is generally good but not uniquely geared to any investor’s wants or needs in a given area. From here we wanted to construct a system, or in this case an Index, that would allow the user to not only customize what they wanted to look at through Folium but to also rate an area for desirability based on attributes. 

### How are we easily conveying an areas desirability to an investor?

Once we had a story-telling map such as Folium in our arsenal we focused on we were going to rate each area based on all the attributes we had compiled. Our solution was simple: For every attribute’s data set, down a column over all the FIPS, assume a normal distribution. From here we could rank each instance of our data (i.e. 10th percentile, 98th percentile) and scale this number to the range 0 – 10. From here we would average all the attributes, across the row for one given FIPS, to produce a number that would essentially ‘rate’ an area on a scale from 0 – 10 (refer to reports or Google Slides the equation that was used). Any negative attributes that would not be desirable given that, unlike a positive attribute, a higher percentile is a bad indicator we would subtract this value from 10 essentially flipping its meaning from negative to positive. 
Once this index was successfully implemented the bigger question was whether an investor cared more about one attribute versus another. This lead us to assume our weights on our index’s attributes would need to be altered on a case by case basis. It is from here we further developed our equation for the so-called Rent Risk Index to incorporate a weight average correctly with panel’s interact function. 
Our final product here was an index that allowed the user to customize the desirability of a given area given that an investor weights certain aspect of said area heavier than others. The output was a number, again, on a scale of 1 – 10, that would show the user more-desirable areas. More specifically, the output, combine with Folium’s Choropleth, showed a color scale on area that would be more or less desirable. 
For a better view of this project’s final product, please refer to the website’s tabs ‘RRI’ and ‘RRI2’ where two different static values of our RRI on Poverty, Property Value, SVI, Food Desert and School ratings based on weights of [1,1,1,1,1] and [70, 30, 10, 100, 100], respectively, were plotted. The values were arbitrary and given as an example for the proof-of-concept. 
