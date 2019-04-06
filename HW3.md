HW3\_Q1
================

Q1 Model selection and regularization: green buildings
------------------------------------------------------

### 1) predictive model for price

In order to find the best predictive model possible for price, we use the forward stepwise procedure. We dicide to use the green\_rating rather than LEED and Energystar as the dummy variable to represent whether the building is green or not. And We set the null model and full model first, try to find which model with variables is the best one to predict price.

    ## lm(formula = Rent ~ cluster_rent + size + class_a + class_b + 
    ##     cd_total_07 + age + leasing_rate + Precipitation + net + 
    ##     empl_gr + Electricity_Costs + Gas_Costs + amenities + green_rating + 
    ##     hd_total07 + cluster_rent:size + size:leasing_rate + size:class_a + 
    ##     size:Precipitation + class_b:age + class_a:age + cd_total_07:net + 
    ##     cluster_rent:leasing_rate + cluster_rent:Electricity_Costs + 
    ##     class_a:empl_gr + cluster_rent:age + age:Electricity_Costs + 
    ##     class_b:Gas_Costs + size:cd_total_07 + cluster_rent:class_a + 
    ##     cluster_rent:class_b + empl_gr:Electricity_Costs + Electricity_Costs:amenities + 
    ##     cluster_rent:amenities + cd_total_07:age + cluster_rent:net + 
    ##     cluster_rent:cd_total_07 + amenities:green_rating + Precipitation:hd_total07 + 
    ##     Gas_Costs:amenities + Precipitation:amenities + size:class_b + 
    ##     size:amenities + class_b:amenities + age:Precipitation + 
    ##     size:age + Precipitation:Gas_Costs + age:Gas_Costs + age:green_rating, 
    ##     data = green)

According to the result we found, the model with 50 terms showed above is the best one.

### 2) overall green certificate effect

Quantify the average change in rental income per square foot associated with green certification, holding other features of the building constant.

According the best predictive model possible for price that we found in the part 1, we want to find the coefficient between price and green\_rating.

    ##                    (Intercept)                   cluster_rent 
    ##                  -2.308033e+01                   1.034758e+00 
    ##                           size                        class_a 
    ##                  -2.388295e-06                   1.019139e+01 
    ##                        class_b                    cd_total_07 
    ##                   1.099334e+01                   1.441856e-03 
    ##                            age                   leasing_rate 
    ##                   4.731505e-02                  -3.374638e-02 
    ##                  Precipitation                            net 
    ##                   4.337446e-01                  -4.758194e-01 
    ##                        empl_gr              Electricity_Costs 
    ##                   4.870871e-01                   7.343143e+01 
    ##                      Gas_Costs                      amenities 
    ##                   8.059079e+02                  -4.491515e+00 
    ##                   green_rating                     hd_total07 
    ##                   1.548000e+00                   1.318899e-03 
    ##              cluster_rent:size              size:leasing_rate 
    ##                   6.108457e-07                   1.024308e-07 
    ##                   size:class_a             size:Precipitation 
    ##                  -1.285704e-05                  -1.462696e-07 
    ##                    class_b:age                    class_a:age 
    ##                  -4.385046e-02                  -2.904877e-02 
    ##                cd_total_07:net      cluster_rent:leasing_rate 
    ##                   6.582872e-04                   1.336617e-03 
    ## cluster_rent:Electricity_Costs                class_a:empl_gr 
    ##                   9.265518e-01                   4.509677e-02 
    ##               cluster_rent:age          age:Electricity_Costs 
    ##                  -2.881894e-03                   2.079773e+00 
    ##              class_b:Gas_Costs               size:cd_total_07 
    ##                  -3.163863e+02                  -1.341105e-09 
    ##           cluster_rent:class_a           cluster_rent:class_b 
    ##                  -1.551886e-01                  -1.129189e-01 
    ##      empl_gr:Electricity_Costs    Electricity_Costs:amenities 
    ##                  -1.782429e+01                   9.388745e+01 
    ##         cluster_rent:amenities                cd_total_07:age 
    ##                  -6.086407e-02                  -8.661721e-06 
    ##               cluster_rent:net       cluster_rent:cd_total_07 
    ##                  -1.008044e-01                  -5.954506e-05 
    ##         amenities:green_rating       Precipitation:hd_total07 
    ##                  -2.346791e+00                  -3.169594e-05 
    ##            Gas_Costs:amenities        Precipitation:amenities 
    ##                   4.185141e+02                  -5.421548e-02 
    ##                   size:class_b                 size:amenities 
    ##                  -9.007804e-06                   2.958987e-06 
    ##              class_b:amenities              age:Precipitation 
    ##                   9.698965e-01                   1.284985e-03 
    ##                       size:age        Precipitation:Gas_Costs 
    ##                  -4.903639e-08                  -2.369091e+01 
    ##                  age:Gas_Costs               age:green_rating 
    ##                  -4.285051e+00                   3.599147e-02

As shown above, the coefficient of green\_rating is 1.548, the coefficient of interaction amenities*green\_rating is -2.35, the coefficient of interaction age*green\_rating is 0.036.

Thus, the building with amenity and green certification will has a rent 0.799 lower than the one only with amenity. the building without amenity but with green certifcation has a rent 1.548 higher than the one neither has amenity nor green certification.

In addition, the building with one year age higher, will get rent 1.5834 higher with the green certification than the one without green certification.

### 3) green certificate effect across buildings

We used cv lasso to demonstrate that green certificate effect is roughly similar across most buildings. We interacted green rating certification with building id to look for the independent certification effect on each building. By using cv lasso, we selected the non-zero coefficients, representing the significant individual effect.

Below is the histogram of the individual effect of certification on rent (in percentage term). We can see that for nearly all of the buildings, having a green rating certification will increase the rent by around 16%. The effect is roughly similar across all buildings.

![](HW3_Q1_files/figure-markdown_github/unnamed-chunk-6-1.png)

Q2 What causes what?
--------------------

1.  You cannot just get data from a few different cities and run the regression of crime rate on number of police officers in a city to understand how more cops in the streets affect crime because correlation is different from causation. Any correlation we find with this regression does not tell us any direction of causation. With the data, we cannot differentiate between a larger police presence causing crime or more crime causing a larger police presence. High crime cities have an incentive to hire more cops so it is likely the result would be a positive correlation between crime and police. Found that when extra police were there for terrorist reasons. This is a way to established a causal relationship in this area.

2.  The researches from the University of Pennsylvania were able to isolate this effect by looking at an example in Washington D.C. They needed to find an example where there is a large amount of police in an area for reasons unrelated to crime. When there is a high risk of terrorism (orange) in D.C., there is an increased police presence by law. Total daily crime decreases on these orange-alert days, we see this with the negative coefficient on high alert that is statistically significant at the 5% level.

3.  They had to control for Metro ridership because of the fewer tourist hypothesis. This was trying to capture the lower ridership on the Metro (people not going out when there is a higher risk) and the lower crime (unrelated to more police). Still controlling for this, there is larger amounts of police is negatively related to crime.

4.  From Table 4, we are looking at if high alert days impact the amount of crime differently in different areas of Washington D.C. The authors use interactions between the high-alert and the location. District 1 (the most likely location of potential terrorism so the most police officers) is the only location where the impact is notable. In other districts, we see the impact as negative or even nonexistent (zero).
