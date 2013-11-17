Bullwhip Effect
===============

This is the beginning set of scripts and tools for graph based time series modeling using python and R. 
My goal for the project is to be able to use these scripts to answer (and bet on) strategic and
logistics questions relating to the bullwhip effect.

You can read about the history of the bullwhip effect at:

http://en.wikipedia.org/wiki/Bullwhip_effect

The bullwhip concept first appeared in Jay Forrester's 1958 article, "Industrial Dynamics—A Major Breakthrough
for Decision Makers," in Harvard Business Review.  The decline of a business was (and still is) often blamed 
on a bad economy or a competitor.  What Forrester sought to model with system dynamics was the process of feedback loops in an industry. 

General Electric, in the 1950’s, was puzzled as to why their household appliance plants sometimes worked three or four shifts and then, a few years later, had to lay of half their staff.

Forrester, after finding out how the corporation made hiring and inventory decisions sought to tackle the problem using system dynamics and simulation--originally using just a pencil and a few pages in a notebook to get an idea of information flow through the company. Through modeling the process (that eventually became computerized for simulation), Forrester accounted how individual managers at the level of sales , distribution and production responded to the information locally available to them as they tried to control their piece of the organization.
Forrester found the managers in each link, in what today is called a supply chain, were responding in a rational fashion to the incentives and information they faced. They were often making choices based on their need to provide good customer service while avoiding excessive inventories. The resulting changes in orders, production, hiring, and other decisions fed back to alter inventories, change prices and advertising rates. The feedback structure of the supply chain amplified each response into persistent cyclical swings, now called the Bullwhip effect. The Bullwhip affect was attributed to exist--even in a state of steady demand--because at every level management relied on their next in chain customer and supplier for demand forecasting. 

##Loading the example Historical Database

 The example I used to build and test the concept was the diffusion of the internet (the product) through countries (the communities)
 The example data is stored in mysql which you can download at:
 
 [MySQL download](http://dev.mysql.com/downloads/mysql/)

   * rate_of_adoption/bass_history.txt 

   The file can be loaded to your MySQL database with the mysql_bass_db_creation.txt script. 
   Be sure to change the location of the file "bass_history.txt" within mysql_bass_db_creation.txt
   to wherever the directory it the file is in on your machine.

##Forecasting Model Pipeline 

The idea behind the rate_of_adoption section is that you have some product and you can get some idea of the expected 
time and volume of adoption. So, the first question is IF it will be adopted (this is a similiarity/recommendation problem)
and the next question is WHEN and HOW MUCH (this is a time series/forecasting problem). 

The prerequisite for IF a product will be adopted is that you build a set of similar product examples (with known diffusion rates)
and then rate the product in question on its similarity to those known products on scale of 1 to 5 (5 being high similarity, 1 being low). 

The prerequisites of time series and forecasting are that you have some community census data on the size of the community (could also
be called the size of the market) and 2 data points (or more) on the percentage adoption within that community. 

There can be some error as to whether a market you're aiming for is a community. If you were marketing to something like an online community, there might
be some question as to whether that community actually functions like one (or is rather as Kurt Vonnegut termed it a [Granfalloon](http://en.wikipedia.org/wiki/Granfalloon)). 
I would argue with Kurt Vonnegut's point of view on nations and say that nations are in fact communities as they adopted the internet together, as shown below. 

One last point is that I use an idea I termed the basic diffusion number to make forecasts. This number (6.67%)
i propose function as the tipping point within a community. Before adoption gets to this point expected growth
is linear. After adoption gets to this point expected growth is exponential. 

####Here are the 5 steps to producing a product diffusion forecast

1  Determine:  Historical similarity of innovation and imitation -> Use python program diffusion_query_branch.py 
to generate a p and q (these are called the imitation and innovation coefficient in the bass model). 

   * You gather past products, calculate their diffusion rate and compare your present product to them. 

2 	Gather Community data: population size, and adoption statistics - >  Use community test.py split data on the 
year where Basic diffusion number = 1 (or is projected to equal 6.67% adoption). 

   * You gather data on the size and past rates of adoption within your target community and project when (and if) you will reach the critical tipping point. 

3 	Execute forecasting programs from that point in time (the basic diffusion number)
using the bass model of diffusion. I benchmark the diffusion forecast versus
Rob J. Hynman's excellent forecast library in R: diffusion_number_bass.r, diffusion_rforecast.r

   * The programs output both csv tables and png images of the forecast 
they save the output forecast tables to text (csv) files and images to png files

4 	Next we go back to the commmunity using the tables. Execute python program population_size.py: This outputs the forecasted adoption at a given year by
combining the projected adoption percentage  with the size of the community. 

   * Example: If its projected  20% adoption in 1999 of 1 billion people in China, the program outputs 200 million. 

5 	Compare accuracy between diffusion_number_bass and diffusion_rforecast or just use to get an estimate 
of adoption via each method at a future time

   This is essentially (for the example of the internet through countries) a train / test comparision


##Future Plans

 I've written an improved similiary engine that uses attributes of items instead of ratings, 
 though it is not yet integrated into this library.
 
 I'll plan to integrate this into the library as and when this is finished.
   
 Expand the historical database to include more examples and be geared to specific industry
 diffusion questions. 
   

##License

Copyright 2013 Luke Otterblad

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
