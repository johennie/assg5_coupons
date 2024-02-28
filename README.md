# Customer Coupon Acceptance - Will a Customer Accept the Coupon?
### Johennie Helton
#### February, 2024


The goal of this project is to identify customers who accepted a driving coupon versus those that did not.
Python was used for analysis and findings which are in the notebooks/assg5_coupons.ipynb file. 
Additionally, matplotlib.pyplot, seaborn, pandas, and numpy libraries were used for computation and visualization.

## 1. Business Understanding
A coupon is delivered to a potential customer's cell phone for a restaurant nearby where s/he is driving. 
We need to analyze and identify the factors that determine whether a driver accepts the coupon once it is delivered to them.

## 2. Data Understanding
During this phase we are to describe and explore the data to make sure it can be used for analysis and visualization in understanding if a coupon would be accepted or not.
Tha data is found in the data/coupons.csv file which contains the following columns:
        
     #   Column                Non-Null Count  Dtype    Description
    ---  ------                --------------  -----    -----------
     0   destination           12684 non-null  object   Driving destination: home, work, or no urgent destination
     1   passanger             12684 non-null  object   Passenger: alone, partner, kid(s), or friend(s)
     2   weather               12684 non-null  object   Weather: sunny, rainy, or snowy
     3   temperature           12684 non-null  int64    Temperature: 30F, 55F, or 80F
     4   time                  12684 non-null  object   Time: 10AM, 2PM, or 6PM
     5   coupon                12684 non-null  object   coupon: Restaurant(<20), Coffee House, 
                                                        Carry out & Take away, Bar, Restaurant(20-50)
     6   expiration            12684 non-null  object   time before it expires: 2 hours or one day
     7   gender                12684 non-null  object   Gender: male, female
     8   age                   12684 non-null  object   Age: below 21, 21 to 25, 26 to 30, etc.
     9   maritalStatus         12684 non-null  object   Marital Status: single, married partner, unmarried partner, 
                                                        or widowed
     10  has_children          12684 non-null  int64    Number of children: 0, 1, or more than 1
     11  education             12684 non-null  object   Education: high school, bachelors degree, associates degree, 
                                                        or graduate degree
     12  occupation            12684 non-null  object   Occupation: architecture & engineering, 
                                                        business & financial, etc.
     13  income                12684 non-null  object   Annual income: less than $12500, $12500 - $24999, 
                                                        $25000 - $37499, etc.
     14  car                   108 non-null    object   car: unknown, Scooter and motorcycle, crossover, Mazda5,
                                                        do not drive, Car that is too old to install Onstar :D
     15  Bar                   12577 non-null  object   Number of times that he/she goes to a bar: 0, less than 1, 1 
                                                        to 3, 4 to 8 or greater than 8
     16  CoffeeHouse           12467 non-null  object   Number of times that he/she goes to a coffee house: 0, 
                                                        less than 1, 1 to 3, 4 to 8 or greater than 8
     17  CarryAway             12533 non-null  object   Number of times that he/she buys takeaway food: 0, less than 1,
                                                        1 to 3, 4 to 8 or greater than 8
     18  RestaurantLessThan20  12554 non-null  object   Number of times that he/she eats at a restaurant with average
                                                        expense less than $20 per person: 0, less than 1, 1 to 3, 
                                                        4 to 8 or greater than 8
     19  Restaurant20To50      12495 non-null  object   Number of times that he/she eats at a restaurant from $20 to 
                                                        $50 per person: 1~3, less1, never, 4 to 8 or greater than 8, etc
     20  toCoupon_GEQ5min      12684 non-null  int64    accepted values: 0, 1
     21  toCoupon_GEQ15min     12684 non-null  int64    accepted values: 0, 1
     22  toCoupon_GEQ25min     12684 non-null  int64    accepted values: 0, 1
     23  direction_same        12684 non-null  int64    accepted values: 0, 1
     24  direction_opp         12684 non-null  int64    accepted values: 0, 1
     25  Y                     12684 non-null  int64    accepted values: 0, 1. If the user will drive there ‘right away’
                                                        or ‘later before the coupon expires’ are labeled as ‘Y = 1’ and 
                                                        answers ‘no, I do not want the coupon’ are labeled as ‘Y = 0’

 

2.1. We checked for missing values

    column                  count
    -------                 ------
    car                     12576
    Bar                       107
    CoffeeHouse               217
    CarryAway                 151
    RestaurantLessThan20      130
    Restaurant20To50          189

2.2. analyzed the data present in the columns of type 'object'
   We noticed that at least 2 columns (age and income) have characters but could be numerical.

2.3. when we described the data we noticed the top values for

    - destination = No Urgent Place

    - passanger = Alone
    
    - coupon = Coffee House
    
    - maritalStatus = Married partner
    
    - education = Some college - no degree
    
    - occupation = Unemployed
    
    - income = 25000− 37499
    
    - car = Scooter and motorcycle
    
    - Bar = never
    
    - CoffeeHouse = less1	
    
    - CarryAway = 1-3
    
    - RestaurantLessThan20 = 1-3	
    
    - Restaurant20To50 = less1



## 3. Data Preparation

3.1. created a new dataframe: modified_df

3.2. replaced NaN, null and/or missing entries with 'unknown' 

3.3. created a datadictionary. I think this will allow us to treat these columns as numerical columns.
    for object columns, created new columns using the datadictionary keys as numerical entries that map to the values 

3.4. converted age column to a numerical column (below21 is now 20, 50plus is now 51). 
    Note that we can always go back to original values via datadictionary and/or original dataframe 

3.5. removed the $ and split income column into income_lowerbound and income_upperbound columns  
    also converted the new income columns to numerical columns. Note that we can always go back to original values 
    via datadictionary and/or original dataframe

### Final Data

        destination	passanger	weather	temperature	time	coupon	expiration	gender	age	maritalStatus	...	dkey_occupation	dkey_income	dkey_car	dkey_Bar	dkey_CoffeeHouse	dkey_CarryAway	dkey_RestaurantLessThan20	dkey_Restaurant20To50	income_lowerbound	income_upperbound
    12198	No Urgent Place	Friend(s)	Sunny	80	10PM	Bar	1d	Female	26	Unmarried partner	...	9	4	0	5	4	2	1	0	50000	62499
    5464	Home	Alone	Sunny	80	6PM	Coffee House	1d	Male	26	Single	...	6	3	0	0	0	2	1	0	75000	87499
    10851	No Urgent Place	Friend(s)	Sunny	80	10PM	Bar	1d	Male	41	Single	...	19	1	0	2	2	2	0	4	62500	74999
    8531	Home	Alone	Snowy	30	6PM	Coffee House	1d	Female	21	Married partner	...	2	2	0	2	3	1	0	2	12500	24999
    12524	No Urgent Place	Friend(s)	Sunny	80	6PM	Restaurant(<20)	2h	Male	21	Unmarried partner	...	21	2	0	2	0	3	0	0	12500	24999
    5 rows × 45 columns


## 4. Data Exploration

In this section we explored the coupon data in general and dove into two coupon type - Bar coupons and CoffeeHouse coupons. 

We use some visualizations to see if there are any differences, and if there is a way to infer if the coupon is accepted.


4.1. Coupon Investigation -  all coupon types

The following categorical plot, generated using the seaborn library, helps us visualize the different coupon types and 
their acceptance (Y==1) or not (Y==0) from the different passanger categories. 

Notice that passanger category = Alone is the top category among all the coupon types.

![coupon_catplot.png](images%2Fcoupon_catplot.png)


In addition, using a seaborn heatmap we can identify the different correlations. 

Notice that coupon type, time, destination, age, and children have a high impact on coupon acceptance.

![coupon_heatmap.png](images%2Fcoupon_heatmap.png)


Finally, using seaborn line and bar plots we differentiate coupon types and their distribution.

![coupon_count_percent_distribution.png](images%2Fcoupon_count_percent_distribution.png)


Both CoffeeHouse coupons and Bar coupons are accepted (Y==1) the most.

4.2 Bar Coupon Investigation - coupon type = Bar

Through a series of acceptance rate calculations and plots we investigate coupons of type = 'Bar'
This seaborn count plot shows the distribution of accepted bar coupons by occupation and gender. 
We also look into coupons of type = Bar that are not Accepted (Y==0) by age and gender, and with and without children. 
Notice that male students accept coupons of type Bar the most.

![bar_coupon_y_1_occupation_gender.png](images%2Fbar_coupon_y_1_occupation_gender.png)


4.3. Coffee House Coupon Investigation - coupon type = CoffeeHouse

Through a series of acceptance rate calculations and plots we investigate coupons of type = 'CoffeeHouse'
This seaborn count plot shows the distribution of accepted bar coupons by occupation and gender. 
We also look into coupons of type = CoffeeHouse that are not Accepted (Y==0) by age and gender, and with and without children.
Notice that unemployed females accept coupons of type CoffeeHouse the most.

![coffee_coupon_y_1_occupation_gender.png](images%2Fcoffee_coupon_y_1_occupation_gender.png)


### Findings
In general, the Coupons of type Coffee House and of type Bar are the top 2 coupon types accepted (Y==1). However, when we breakout by coupon types and look at the passangers, the 'Carry out & take away' coupons are prefered by those with Alone passangers; however, they (Alone passangers) tend to no accept the 'Coffee House' coupons.

#### Coupon Type = Bar

        Male students accept Bar coupons the most. A distance second are males that work in Sales & Related fields
        By Age and Children
        
        In general females 21, 31 and over 50 year olds are more likely not to accept bar coupons. But when children are factored, females of all age groups are more likely to not accept bar coupons when children are present.
        
        In general 21, 26, and 31 yr old males accept bar coupons. But when children are factored, 41 yr old females are more likely to accept Bar coupons when children are present.
        
        The acceptance rate of those who went to a bar 3 or fewer times a month is 49.83700405502107
        
        The acceptance rate of those who went to a bar more than 3 times a month is 7.0525562534785715
        
        The acceptance rate of drivers who go to a bar more than once a month and are over the age of 25 is 13.60769473352255
        
        
### Coupon Type = CoffeeHouse
        
        Unemployed women accept cofee house coupons the most. Closely followed by male students.
        By Age and Children
        
        In general males 21, 26 year old are more likely not to accept the coffee house coupon. But when children are factored, females of all age groups are more likely to not accept bar coupons when children are present.
        
        In general 21 yr old males are likely to accept a coffee house coupon. However, when children are factored, most females (21, 31, 36, 41, over 50 age groups) are more likely to accept coffeeHouse coupons when children are present.
        
        The acceptance rate of those who went to a CoffeeHouse 3 or fewer times a month is 54.36714679742568
        
        The acceptance rate of those who went to a CoffeeHouse more than 3 times a month is 18.163244458065176
        
        The acceptance rate of drivers who go to a CoffeHouse more than once a month and are over the age of 25 is 21.428571428571427
         

## 5. Next Steps
The next steps in this analysis would be modeling and investigating other factors in the data to verify the above findings and identify if there are other factors driving the choice of accepting or not accepting coupons received on your phone while driving nearby a restaurant.

## 6. Conclusion
The choice of accepting a coupon is affected by different factors and cannot be easily predicted. It seems that occupation, gender, and if children are present are factors in the choice.

