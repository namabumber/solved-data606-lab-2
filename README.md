Download Link: https://assignmentchef.com/product/solved-data606-lab-2
<br>
Some define statistics as the field that focuses on turning information into knowledge. The first step in that process is to summarize and describe the raw information – the data. In this lab we explore flights, specifically a random sample of domestic flights that departed from the three major New York City airports in 2013. We will generate simple graphical and numerical summaries of data on these flights and explore delay times. Since this is a large data set, along the way you’ll also learn the indispensable skills of data processing and subsetting.

<h1>Getting started</h1>

<h2>Load packages</h2>

In this lab, we will explore and visualize the data using the <strong><sub>tidyverse </sub></strong>suite of packages. The data can be found in the companion package for OpenIntro labs, <strong><sub>openintro</sub></strong>. Let’s load the packages.

library(tidyverse) library(openintro)

<h2>Creating a reproducible lab report</h2>

Remember that we will be using R Markdown to create reproducible lab reports. In RStudio, go to New File

-&gt; R Markdown… Then, choose From Template and then choose Lab Report for OpenIntro Statistics Labs from the list of templates.

See the following video describing how to get started with creating these reports for this lab, and all future labs:

<a href="https://www.youtube.com/watch?v=Pdc368lS2hk"><strong>Basic R Markdown with an OpenIntro Lab</strong></a>

<h2>The data</h2>

The <a href="http://www.rita.dot.gov/bts/about/">Bureau of Transportation Statistics</a> (BTS) is a statistical agency that is a part of the Research and Innovative Technology Administration (RITA). As its name implies, BTS collects and makes transportation data available, such as the flights data we will be working with in this lab.

First, we’ll view the nycflights data frame. Type the following in your console to load the data:

data(nycflights)

The data set nycflights that shows up in your workspace is a <em><sub>data matrix</sub></em>, with each row representing an <em>observation </em>and each column representing a <em><sub>variable</sub></em>. R calls this data format a <strong><sub>data frame</sub></strong>, which is a term that will be used throughout the labs. For this data set, each <em><sub>observation </sub></em>is a single flight.

To view the names of the variables, type the command

names(nycflights)

This returns the names of the variables in this data frame. The <strong><sub>codebook </sub></strong>(description of the variables) can be accessed by pulling up the help file:

One of the variables refers to the carrier (i.e. airline) of the flight, which is coded according to the following system.

<ul>

 <li>carrier: Two letter carrier abbreviation.

  <ul>

   <li>9E: Endeavor Air Inc.</li>

   <li>AA: American Airlines Inc.</li>

   <li>AS: Alaska Airlines Inc. <strong>– </strong>B6: JetBlue Airways</li>

   <li>DL: Delta Air Lines Inc.</li>

   <li>EV: ExpressJet Airlines Inc.</li>

   <li>F9: Frontier Airlines Inc. <strong>– </strong>FL: AirTran Airways Corporation <strong>– </strong>HA: Hawaiian Airlines Inc.</li>

   <li>MQ: Envoy Air <strong>– </strong>OO: SkyWest Airlines Inc.</li>

   <li>UA: United Air Lines Inc. <strong>– </strong>US: US Airways Inc. <strong>– </strong>VX: Virgin America <strong>– </strong>WN: Southwest Airlines Co. <strong>– </strong>YV: Mesa Airlines Inc.</li>

  </ul></li>

</ul>

Remember that you can use glimpse to take a quick peek at your data to understand its contents better.

glimpse(nycflights)

The nycflights data frame is a massive trove of information. Let’s think about some questions we might want to answer with these data:

<ul>

 <li>How delayed were flights that were headed to Los Angeles?</li>

 <li>How do departure delays vary by month?</li>

 <li>Which of the three major NYC airports has the best on time percentage for departing flights?</li>

</ul>

<h1>Analysis</h1>

<h2>Lab report</h2>

To record your analysis in a reproducible format, you can adapt the general Lab Report template from the <strong>openintro </strong>package. Watch the video above to learn how.

<h2>Departure delays</h2>

Let’s start by examing the distribution of departure delays of all flights with a histogram.

ggplot(data = nycflights, aes(x = dep_delay))


geom_histogram()

This function says to plot the dep_delay variable from the nycflights data frame on the x-axis. It also defines a geom (short for geometric object), which describes the type of plot you will produce.

Histograms are generally a very good way to see the shape of a single distribution of numerical data, but that shape can change depending on how the data is split between the different bins. You can easily define the binwidth you want to use:

ggplot(data = nycflights, aes(x = dep_delay)) + geom_histogram(binwidth = 15)

ggplot(data = nycflights, aes(x = dep_delay))


geom_histogram(binwidth = 150)

<ol>

 <li>Look carefully at these three histograms. How do they compare? Are features revealed in one that are obscured in another?</li>

</ol>

If you want to visualize only on delays of flights headed to Los Angeles, you need to first filter the data for flights with that destination (dest == “LAX”) and then make a histogram of the departure delays of only those flights.

<table width="632">

 <tbody>

  <tr>

   <td width="632">lax_flights &lt;- nycflights %&gt;% filter(dest == “LAX”)ggplot(data = lax_flights, aes(x = dep_delay)) + geom_histogram()</td>

  </tr>

 </tbody>

</table>

Let’s decipher these two commands (OK, so it might look like four lines, but the first two physical lines of code are actually part of the same command. It’s common to add a break to a new line after %&gt;% to help readability).

<ul>

 <li>Command 1: Take the nycflights data frame, filter for flights headed to LAX, and save the result as a new data frame called lax_flights.

  <ul>

   <li>== means “if it’s equal to”.</li>

   <li>LAX is in quotation marks since it is a character string.</li>

  </ul></li>

 <li>Command 2: Basically the same ggplot call from earlier for making a histogram, except that it uses the smaller data frame for flights headed to LAX instead of all flights.</li>

</ul>

<strong>Logical operators: </strong>Filtering for certain observations (e.g. flights from a particular airport) is often of interest in data frames where we might want to examine observations with certain characteristics separately from the rest of the data. To do so, you can use the filter function and a series of <strong><sub>logical operators</sub></strong>. The most commonly used logical operators for data analysis are as follows:

<ul>

 <li>== means “equal to”</li>

 <li>!= means “not equal to”</li>

 <li>&gt; or &lt; means “greater than” or “less than”</li>

 <li>&gt;= or &lt;= means “greater than or equal to” or “less than or equal to” You can also obtain numerical summaries for these flights:</li>

</ul>

<table width="632">

 <tbody>

  <tr>

   <td width="632">lax_flights %&gt;% summarise(mean_dd           = mean(dep_delay),median_dd = median(dep_delay),n                      = n())</td>

  </tr>

 </tbody>

</table>

Note that in the summarise function you created a list of three different numerical summaries that you were interested in. The names of these elements are user defined, like mean_dd, median_dd, n, and you can customize these names as you like (just don’t use spaces in your names). Calculating these summary statistics also requires that you know the function calls. Note that n() reports the sample size.

<strong>Summary statistics: </strong>Some useful function calls for summary statistics for a single numerical variable are as follows:

<ul>

 <li>mean</li>

 <li>median</li>

 <li>sd</li>

 <li>var</li>

 <li>IQR</li>

 <li>min</li>

 <li>max</li>

</ul>

Note that each of these functions takes a single vector as an argument and returns a single value. You can also filter based on multiple criteria. Suppose you are interested in flights headed to San Francisco (SFO) in February:

<table width="632">

 <tbody>

  <tr>

   <td width="632"> </td>

  </tr>

 </tbody>

</table>

Note that you can separate the conditions using commas if you want flights that are both headed to SFO <strong>and </strong>in February. If you are interested in either flights headed to SFO <strong><sub>or </sub></strong>in February, you can use the | instead of the comma.

<ol>

 <li>Create a new data frame that includes flights headed to SFO in February, and save this data frame as sfo_feb_flights. How many flights meet these criteria?</li>

</ol>

68 Flights met the criteria

<ol>

 <li>Describe the distribution of the <strong><sub>arrival </sub></strong>delays of these flights using a histogram and appropriate summary statistics. <strong><sub>Hint: </sub></strong>The summary statistics you use should depend on the shape of the distribution.</li>

</ol>




Another useful technique is quickly calculating summary statistics for various groups in your data frame. For example, we can modify the above command using the group_by function to get the same summary stats for each origin airport:

<table width="632">

 <tbody>

  <tr>

   <td width="632"> </td>

  </tr>

 </tbody>

</table>

Here, we first grouped the data by origin and then calculated the summary statistics.

<ol>

 <li>Calculate the median and interquartile range for arr_delays of flights in in the sfo_feb_flights data frame, grouped by carrier. Which carrier has the most variable arrival delays?</li>

</ol>

<table width="632">

 <tbody>

  <tr>

   <td width="632"> </td>

  </tr>

 </tbody>

</table>

<h2>Departure delays by month</h2>

Which month would you expect to have the highest average delay departing from an NYC airport? Let’s think about how you could answer this question:

<ul>

 <li>First, calculate monthly averages for departure delays. With the new language you are learning, you could

  <ul>

   <li>group_by months, then</li>

   <li>summarise mean departure delays.</li>

  </ul></li>

 <li>Then, you could to arrange these average delays in descending order</li>

</ul>

<table width="632">

 <tbody>

  <tr>

   <td width="632"> </td>

  </tr>

 </tbody>

</table>

<ol>

 <li>Suppose you really dislike departure delays and you want to schedule your travel in a month that minimizes your potential departure delay leaving NYC. One option is to choose the month with the lowest mean departure delay. Another option is to choose the month with the lowest median departure delay. What are the pros and cons of these two choices?</li>

</ol>

The mean would account for the outliers while the median would not account for the outliers since the median only give you the number that is in the middle of the dataset. Therefore it would be better to use the mean in this situation as it would give the passengers the true departure delay accounting for outliers. July had the highest average departure delay.

<table width="632">

 <tbody>

  <tr>

   <td width="632"> </td>

  </tr>

 </tbody>

</table>

The highest median departure delay month is December

Mean would be a more reliable measure to decide which month to fly as mean accounts for the outliers while the Median only the value of the middle of the dataset.

<h2>On time departure rate for NYC airports</h2>

Suppose you will be flying out of NYC and want to know which of the three major NYC airports has the best on time departure rate of departing flights. Also supposed that for you, a flight that is delayed for less than 5 minutes is basically “on time.”” You consider any flight delayed for 5 minutes of more to be “delayed”.

In order to determine which airport has the best on time departure rate, you can

<ul>

 <li>first classify each flight as “on time” or “delayed”,</li>

 <li>then group flights by origin airport,</li>

 <li>then calculate on time departure rates for each origin airport,</li>

 <li>and finally arrange the airports in descending order for on time departure percentage.</li>

</ul>

Let’s start with classifying each flight as “on time” or “delayed” by creating a new variable with the mutate function.

nycflights &lt;- nycflights %&gt;%

mutate(dep_type = ifelse(dep_delay &lt; 5, “on time”, “delayed”))

The first argument in the mutate function is the name of the new variable we want to create, in this case dep_type. Then if dep_delay &lt; 5, we classify the flight as “on time” and “delayed” if not, i.e. if the flight is delayed for 5 or more minutes.

Note that we are also overwriting the nycflights data frame with the new version of this data frame that includes the new dep_type variable.

We can handle all of the remaining steps in one code chunk:

<table width="632">

 <tbody>

  <tr>

   <td width="632">nycflights %&gt;% group_by(origin) %&gt;%summarise(ot_dep_rate = sum(dep_type == “on time”) / n()) %&gt;% arrange(desc(ot_dep_rate))</td>

  </tr>

 </tbody>

</table>

<ol>

 <li>If you were selecting an airport simply based on on time departure percentage, which NYC airport would you choose to fly out of?</li>

</ol>

<h1>More Practice</h1>

<ol>

 <li>Mutate the data frame so that it includes a new variable that contains the average speed, avg_speed traveled by the plane for each flight (in mph). <strong><sub>Hint: </sub></strong>Average speed can be calculated as distance divided by number of hours of travel, and note that air_time is given in minutes.</li>

 <li>Make a scatterplot of avg_speed vs. distance. Describe the relationship between average speed and distance. <strong><sub>Hint: </sub></strong>Use geom_point().</li>

</ol>

The avg_speed vs Distance is exponential. When the average speed becomes greater the distance also becomes a lot greater. This means that destinations that are farther way fly at faster speeds.

<ol>

 <li>Replicate the following plot. <strong><sub>Hint: </sub></strong>The data frame plotted only contains flights from American Airlines, Delta Airlines, and United Airlines, and the points are colored by carrier. Once you replicate the plot, determine (roughly) what the cutoff point is for departure delays where you can still expect to get to your destination on time.</li>

</ol>

ggplot(dl_aa_ua, aes(dep_delay, arr_delay, color = carrier)) + xlim(-20, 100) + ylim(-20, 100) + geom_po

In order to still arrive on time the airplane can depart at most around 40 minute late.