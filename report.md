#### # Capstone Project - The Battle of Neighborhoods

## Introduction/Business Problem


#### "Help investor to find the most suitable restaurant to invest?"  

Vancouver is right closed to the sea therefore it's quite famous for its seafood. My boss, an American investor just moved here was amazed by that.  Then he decided to __become a shareholder of top Seafood restaurant__ .

He has tried top 3 seafood restaurants elected by OpenTable and rated them. He asked me to investigate the top 4-6 restaurants and sent him a report to help him make decisions. 

He told me his ideal restaurant should 
- have some nightlife spots nearby 
- be closed to some bus stop or metro stations
- have some shopping malls around

He wants me to concern about the nearby environment around restaurants as that will somehow influence the popularity of restaurants. Diners can not only come to enjoy great dinners but also find something fun to do around.

People who wants to invest on some restaurants or open new restaurants can follow the ideas of this report. This report is going to introduce an executable scheme to solve the problem.

## Data

So basically my stakeholder wants a suggestion. Therefore, we can build a recommendation system to do the job. 

The data all we need is his ratings to the restaurant, information about the candidate restaurants and nearby venues around the reference restaurants and  candidate restaurants. 

### Basic data about stakeholder's choice and candidates
Data below is collected from the Opentable. 

#### Stakeholder's ratings 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>lat</th>
      <th>lon</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Ancora Ambleside</td>
      <td>49.327349</td>
      <td>-123.154458</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Boulevard Kitchen &amp; Oyster Bar</td>
      <td>49.282718</td>
      <td>-123.123883</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Oddfish Restaurant</td>
      <td>49.270894</td>
      <td>-123.147585</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>

#### Candidate restaurants

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Address</th>
      <th>lat</th>
      <th>lon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Ferris' Upstairs Seafood and Oyster Bar</td>
      <td>48.427011</td>
      <td>-123.369044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joe Fortes Seafood &amp; Chop House</td>
      <td>49.284791</td>
      <td>-123.124556</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Blue Crab Seafood House</td>
      <td>48.421903</td>
      <td>-123.379851</td>
    </tr>
  </tbody>
</table>

###  Count of nearby venues 

Data of nearby venues are collected using FourSquare API. The detailed information is stated in the Data Wrangling notebook. 

####  count of venues around stakeholder's choice
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>30</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>17</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

####  count of venues around candidate restaurants
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>30</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

## Methodology
We need to build a  recommendation algorithm to solve the problem.  As the stakeholder has given us his ranking list, the content-based recommendation comes to be the best choice. 

Combine with FourSquare API which provides how many venues in different category of seafodd restaurants, a matrix which captured characteristic of venues nearby restaurants are built. Stakeholder's favourite list is the profile to combine with the matrix to become a weighted matrix of favourite restaurants.

The weighted matrix can be applied on 3 candidate restaurants with venues information to generate a ranking result. the top one on the ranking list can be recommended to the stakeholder. 

So for the first step, we use the FourSquare API to get venues near stakeholder's preference. 

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>30</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>17</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

Next, we are going to discover venues near candidate restaurants. 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>30</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

We need to normalize these two dataframes. 

####  stakeholder's choice    -  count of nearby venues 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>0.23</td>
      <td>0.0</td>
      <td>0.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>1.00</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.56</td>
      <td>0.2</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>

####  candidate restaurants     -  count of nearby venues 

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nightlife spot</th>
      <th>bus stop</th>
      <th>metro station</th>
      <th>shopping mall</th>
    </tr>
    <tr>
      <th>restaurant</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>0.67</td>
      <td>0.17</td>
      <td>0.5</td>
      <td>0.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.00</td>
      <td>0.10</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>

For the next step, we are going to build the user profile using stakeholder's rating and stakeholder's choice    -  count of nearby venues . 
#### User Profile 
    nightlife spot    5.00
    bus stop          7.93
    metro station    5.80
    shopping mall    6.80
    dtype: float64

Now we get the user profile. For the next step, we will apply it to the candidate dataframe to get a weighted preference matrix. 

## Results 
     restaurant
    1    0.457427
    2    1.000000
    3    0.031061
    dtype: float64

Here is the final result that we get from the content-based recommendation system. Here we can see the second restaurant score the highest score and we recommend the second restaurant to the stakeholder. 

The restaurant named Joe Fortes Seafood & Chop House has 6 nightlife spots, 30 bus stops, 3 metro stations and 5 shopping malls around. This environment ensures the  diners can enjoy their nights afrer dinner. 

## Discussion 
We can see that the chosen restaurant has got a wired value 1. I re-checked the number and ensure the correctness of the result. And that's the correct result. I guess that's the consequence of not having a large data sample. I need to consider more categories of venues to get more accurate results. 

## Conclusion 
Finally, the problem is resolved.  In this case, the stakeholder planned to invest on a seafood restaurant. He listed out 3 sea restaurants and rated them. He asked me to analyze Opentable 's top 4- 6 seafood restaurants and recommend him one to invest. As we have got his rating list, we utilized Content-based recommendation algorithm. Through building a User Profile, we are able to derivate final ratings of candidate restaurants. Joe Fortes Seafood & Chop House is the one that we recommend to the stakeholder. 