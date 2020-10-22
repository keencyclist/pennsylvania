# Pennsylvania Bicycle-Motor Vehicle Collisions


## Project Description
Bicyclists and would-be bicyclists are often concerned with the risk of injury. Yet what do we  know about bicycling crashes and injuries? Even though every state and the U.S. DOT's National Highway Traffic Safety Administration keep databases of police-reported road crashes, the data with regard to bicycling is limited and therefore sometimes misleading. Most bicycling injuries are excluded from these databases, because crashes are only reportable if they involve a motor vehicle in transport. Collisions between a bicyclist and a pedestrian, animal, or other bicyclist, or a single-bicycle crash (a fall or a collision with a fixed object) are excluded, even though hospital data shows that they account for the majority of emergency department visits by bicyclists. Furthermore, bicyclists are often treated differently in the crash reporting forms or crash database compared to motorists, which means that key fields with regard to crash circumstance and operator behavior are not reported, not coded, or not coded clearly. Sometimes bicyclists are included with pedestrians as "non-motorists," leading to confusing coding such as "jaywalking" bicyclists.

Often what we would really like to know is the <i>crash risk</i> associated with different locations, road designs, or operator behaviors. But risk is outcome divided by exposure, and there are very few public sources estimating the quantity of bicycling under different circumstances. For example, we would need to know the share of bicycle travel that occurs in daylight and after dark understand the relative risk of night-time bicycling.

Despite these limitations, we can learn a good deal about bicycle-motor vehicle collisions (BMVCs) from crash reports, if we find data sources that contain key attributes. We can look at crash prevalence: where and under what circumstances do BMVCs occur? In general we can assume that the distribution of crashes is primarily a function of to the distribution of bicycle distance traveled: roads with many bicycle crashes likely have many bicyclists. Even if prevalence is not informative about individual crash risk, it does give us information about where the location and circumstances of the collective problem.

We can also look at the variation in the severity of injuries by modeling injury severity, given that a crash has occurred. This is not the same thing as modeling injury crash occurrence, but it can help us understand what factors make crashes more severe.

My larger project makes use of several available crash databases for U.S. states and from U.S. DOT/NHTSA. This current analysis uses a single source, Pennsylvania reported crashes, to look at crash prevalence by type of location (speed limit and urban or rural), intersection or midblock, manner of collision, and other features, and also to create an injury severity model.

The ultimate purpose of this research is to guide the implementation of countermeasures that could improve bicyclist safety. This topic was addressed in a [National Transportation Safety Board bicycling report](https://aashtojournal.org/2019/12/13/ntsb-releases-bike-safety-recommendations-report/) released in December 2019. I  compare my results on collision types and locations and injury severity to some of the data and modeling in this NTSB
report, particularly their recommendation for infrastructure such as separated bike lanes.

## Data Source
The data for this analysis comes from PennDOT, which provides [20 years of annual crash data](https://pennshare.maps.arcgis.com/apps/webappviewer/index.html?id=8fdbf046e36e41649bbfd9d7dd7c7e7e) for the state of Pennsylvania. I downloaded the annual files for 1999 to 2019. (The files for 2001 were missing and repeated requests to PennDOT to make them available were not successful.) PennDOT provides several files for each year, representing separate tables from a relational database linked by key fields. The tables include Crash, Person, Vehicle, and Roadway. Unlike virtually every other state crash database, bicyclists are coded in the same manner as motor vehicle occupants. Bicycles are included in the Vehicle file (as VEH_TYPE = 'Bicycle' or 'Other Pedalcyclist'). Therefore Manner of Collision (angle, rear-end, sideswipe, backing, etc.) and Vehicle Movement (straight, left, right, etc.) are coded for bicyclists.

The final dataset I used contains 29,726 bicyclists and 30,090 motorists involved in 29,489 crashes.

## Crash Prevalence

#### Location Characteristics

More than 80% of Pennsylvania bicycle-motor vehicle collisions (BMVCs) occur in urban areas. Two-thirds occur in locations where the posted speed limit is 25 mph or less. Although lower-speed roads predominate, about a quarter of BMVCs occur where the speed limit is 30-35 mph, mostly in urban areas. Only 10% of BMVCs occur where the posted speed limit is 40 mph or more, and of those, about 40% occur in rural areas.

**Crash Prevalence by Speed Limit and Urban/Rural: All Crashes**
![Posted Speed Limit and Urban/Rural](/images/SpeedLimitUrban_Rural.png)

This picture changes significantly if we consider only crashes that resulted in serious or fatal injuries. Only half of these serious or fatal BMVCs are on streets with a speed limit of 25 mph or less, 27% with a limit of 30-35 mph, and another 27% with a limit of 40 mph or more. Rural areas are also disproportionately represented compared to the distribution of all crashes. Fatal crashes are even more skewed to high-speed streets and rural areas: 44% of fatal BMVCs occur in rural area (vs 19% of all BMVCs) and 66% occur where the speed limit is 30 mph or greater (vs. 32% of all).

**Crash Prevalence by Speed Limit and Urban/Rural: Serious and Fatal Injury Crashes**
![Posted Speed Limit and Urban/Rural](/images/SpeedLimitUrban_Rural_Serious.png)

What about the location of crashes within the roadway system? The Pennsylvania data coding distinguishes between crashes that happen inside an intersection (the area shared by more than one roadway) and crashes that are intersection-related (crashes that are related to turning and crossing movements that generally happen within 100 feet of the actual intersection). I have combined intersection and intersection-related crashes. Driveways, even large commercial ones with traffic signals, are not counted as intersections. I used a separate feature to identify driveways (the category also includes parking lot collisions). Functionally, driveways are the same as intersections.

As shown in the figure below, 28% of all BMVCs occurred between intersections ("midblock"). The remaining 72% were at intersections (or intersection-related) or driveways. Four-way intersections were by far the most common, but T-intersections were also a significant portion.

**Crash Prevalence by Intersection Type / Midblock**
![Intersection vs. Midblock](/images/Intersection.png)

If we consider only crashes that produced serious or fatal injuries, 40% were midblock. For fatal crashes only, 50% were midblock. Clearly midblock crashes are more likely to be serious. Nevertheless, the majority of serious crashes were at intersections or driveways, and for fatalities, crash prevalence is evenly split between the two types of locations.

#### Crash Characteristics
The PennDOT database contains "Manner of Collision" for BMVCs. As shown in the figure below nearly 70% were angle collisions. The next most common were sideswipe and rear-end collisions. The head-on and sideswipe (opposite direction) collision types imply that either the bicyclist or motorist was on the wrong side of the road.

**Manner of Collision: All Crashes**
![Collision Type](/images/CollisionType.png)

Considering serious and fatal injury BMVCs only as shown in the figure below, we find that angle collisions still predominate. Rear-end collisions increase in importance, but only to 11% of the total. Head-on collisions also increase in importance. Considering fatal injuries only, rear-end collisions rise in importance to 18%, but the majority, 58%, are still angle collisions.

**Manner of Collision: Serious and Fatal Injury Crashes**
![Collision Type](/images/CollisionType_Serious.png)

Most angle BMVCs (83% of them) happen at intersections or driveways, and most rear-end BMVCs (64% of them) happen midblock. However, only a small share of midblock collisions are rear-end collisions: 41% are angle (including cases where the bicyclist is crossing a street between intersections), 27% are same-direction sideswipes, 14% are rear-end, 7% are head-on, 4% are opposite direction sideswipes, and the remaining 7% are other or unknown. Rear-end and same-direction sideswipe collisions are a higher share of *serious and fatal* BMVCs (23% and 17% respectively), but angle collisions represent 36% of these midblock BMVCs.

#### Prevalence of Rear-end and Sideswipe Collisions
The NTSB bicycle safety report recommends separated bike lanes as the primary method to reduce serious collisions, arguing that they can "all but eliminate hit from behind, overtaking, and sideswipe crashes." However, rear-end and sideswipe collisions represent only 18% of serious and 27% of fatal BMVCs in Pennsylvania. Moreover, separated bike lanes are generally installed in the areas with the highest density of bicycling: city streets with posted speed limits of 30 mph or less. As of 2019, all of the separated bike lanes in Pennsylvania are in Philadelphia, Pittsburgh, and Lancaster, none on roads with a speed limit greater than 30 mph (almost all 25). On these types of urban roads, the targeted collision type is an even smaller share of the problem: only 12% of serious and 11% of fatal BMVCs.

Out of the 325 bicyclist fatalities in this dataset, only 11 were rear-end or sideswipe collisions on low-speed (<=30 mph) urban roads where the motorist was the striking party. Of these, five occurred at night, when the primary factor is visibility, particularly since many bicyclists do not use lights at night (the Pennsylvania data records use of a rear reflector but not use of a rear light). Of the six that occurred in daylight, three involved criminally dangerous driving:

* In October 2015 on Forbes Avenue in Pittsburgh, a motorist who had been using marijuana struck a stopped car  that then struck a stopped bicyclist who was a college professor. The driver, who fled the scene, was sentenced to 5-10 years.
* In March 2006, a legally blind driver drifted into the shoulder of Boalsburg Road in State College, killing a bicyclist who was also a college professor.
* In July 2015, a 21 year old, unlicensed driver accelerated rapidly to pass a car stopped in the lane in front of her on N. 2nd St, Philadelphia, which has one narrow lane in each direction. After passing the car, she struck a bicyclist who had been in front of the stopped car. She fled the scene and removed the car's plates, but was caught and charged with homicide.

The other three crashes do not seem to be what we would consider rear-end crashes:

* In June 2000, a bicyclist was killed at the intersection of Frankford Avenue and Ashburner Street in Philadelphia, a signalized T intersection. A motorist, whose impairment is unknown, was driving in the left lane and rear-ended a stopped car. One of the two motor vehicles hit the left side of the bicyclist, who was in the right lane.
* In November 2003, a bicyclist in a bike lane next to on-street parking was killed on Torresdale Avenue near East Howell Street. A passenger car, whose movements are unknown, was involved in a "non-collision" -- perhaps starting to move out from parking or opening a door. The bicyclist fell underneath a heavy truck going in the same direction.
* In August 2009, a bicyclist was struck and killed by a large truck at the intersection of Meyran Avenue and Luisa Street in Pittsburgh, a 4-way stop intersection of two narrow streets. The bicyclist's position, movement, and direction of travel are unknown. The driver, who fled the scene, was coded as "distracted."



#### Vehicle and Operator Characteristics

What types of motor vehicles are involved in BMVCs? About two-thirds were passenger automobiles and 30% were light trucks (SUVs, vans, pickups, and other small trucks). Only 1% were large trucks and another 1% were buses (mostly transit buses). When we look at serious and fatal injuries, the small truck share rises from 9% to 14% and the large truck share rises from 1% to 4%. Considering fatal BMVCs only, the small truck share is 15%, the large truck share is 7%, and the bus share is 2%.

**Motor Vehicle Type**
![Motor Vehicle Type](/images/MotorVehicleType.png)

Fewer than 2% of drivers involved in BMVCs were impaired (mostly by alcohol or drugs). When we look at serious or fatal injury crashes, this rises to 6%, and for fatal crashes alone, 15%. Only 1.4% of drivers were speeding overall, but 4% were for serious or fatal crashes and 7% were for fatal crashes alone.


## Injury Severity Model
The injury severity for the roughly 30,000 bicyclists involved in crashes in this dataset is shown in the chart below. Since these are not medical records, there is often uncertainty about the extent of injuries. Nearly half were coded as "possibly injury" and another 19% were coded as "injury of unknown severity." Just over 1% had a fatal injury and 5.5% had a suspected serious, but non-fatal, injury.
![Model 1: Complete Data](/images/InjurySeverity.png)

I decided to do a simple binary model, comparing an outcome of serious or fatal injury with all other outcomes (no injury or minor, possible, or unknown injury). I decided against a multi-class model, for the following reasons:
* a binary model is easier to interpret
* it would be useful to have separate classes for fatal and serious but non-fatal injuries, but there were only 325 fatalities, making it harder to find statistically significant results
* among those cases that are not serious or fatal, there are many that were "possible" or "unknown" injury severity, adding uncertainty into the categories.
* the model is comparable to the one created for the [National Transportation Safety Board bicycling report](https://aashtojournal.org/2019/12/13/ntsb-releases-bike-safety-recommendations-report/) released in December 2019.

I created a logistic regression model using the Statsmodels Python library, with a dependent variable of Serious or Fatal injury (coded as 1) or any other injury status (coded as 0). Exponentiating each coefficient provides the odds associated with that feature. All of the features were categorical, with one category left out of the model, so the odds represents the odds ratio *compared to the left-out or "reference" category*. An odds ratio of 1 means that there is no difference between the modeled category and the reference category. An odds ratio greater than 1 means that the modeled category has a positive effect compared to the reference, and an odds ratio less than 1 means that it has a negative effect. I calculated the 95% confidence interval associated with each estimated odds ratio: if the interval includes 1,  there is not enough information to determine if there is a positive or negative effect.

Although the direction of effect (positive, negative, or none) is easy to understand, the magnitude of an odds ratio has no intuitive interpretation. However, for factors that are uncommon, the odds ratio is a reasonable approximation of the *relative risk*, which does have an easy interpretation: a relative risk of 2, for example, means that the factor is twice as risky as the reference. I should emphasize that injury severity models help us understand the risk of a serious or fatal injury, *given that a crash has occurred*, but provide no information about the risk of a crash occurring.

The odds ratios and their confidence intervals for the first model, using the complete dataset, are plotted below.


**Model 1: Complete Dataset**
![Model 1: Complete Dataset](/images/ORs_all.png)

Location Characteristics
* Rural areas have an odds ratio (OR) of 2.0 compared to urban areas, meaning that the risk of serious or fatal injury is higher than in urban areas, even controlling for all the other features in the model.
* The risk of serious injury increases with higher posted speed limits. The highest category, 50+ mph, has an OR of 2.1 compared to the reference category of 25 mph or less.
* Midblock locations have an OR of 1.5 compared to intersections.

Vehicle Type
* The involvement of a bus has an OR of 2.4.
* I included the involvement of a small trucks as a feature, as well as the interaction between small truck and midblock location. The portion representing crashes involving Small Trucks not at midblock (i.e., at intersections) had an OR of 1.8.
* As with small trucks, the interaction between large truck and midblock was included. Again, the midblock portion was not significant but the truck / intersection effect had an OR of 5.3, the highest of any feature in the model.

Lighting Conditions

The reference category was daylight. In comparison:
* dark conditions (which are mostly lit but include a few with unknown lighting) had an odds ratio of 1.5
* dark and unlit conditions had an OR of 1.8
* crashes at dawn had an OR of 2.4
* there was no evidence that crashes at dusk were more serious than those in daylight.

Collision Type

Compared to angle collisions (by far the most common type)
* sideswipe (same direction) collisions were less likely to be serious (OR of 0.5)
* sideswipe (opposite direction) collisions were also less likely to be serious, but the effect was just short of being statistically significantly
* rear-end collisions were somewhat more likely to be serious, with an OR of 1.2
* head-on collisions were more likely to be serious, with an OR of 1.5
* all the other collision types did not have a statistically significant effect, perhaps in part because they were rare.

Crashes in which the bicyclist was struck (rather than striking) were more likely to be serious, with an OR of 1.2.

Operator Behavior
* Crashes that were alcohol-related were more likely to be serious (OR of 2.3).
* Similarly, speeding-related crashes had an OR of 2.4.
* The largest effect was for crashes where the driver was using drugs (OR of 3.4).
* Bicyclists using a helmet were somewhat more likely to have serious injuries (OR of 1.2), contrary to what would be expected. This result may be due to poor coding: 12% of the bicyclists were wearing helmets, 41% were not wearing helmets, and the remaining 48% had unknown helmet use. Therefore the reference group (including both 'no' and 'unknown') may have had many helmet-users.
* Hit and run crashes were associated with lower odds of serious injury, contrary to my expectations.

Although not shown in the OR plots, the models also include bicyclist age and gender. Female bicyclists were slightly less likely to have serious injuries (OR of 0.8). Only three age groups were statistically different from the other age groups: 20 to 29 year olds were less likely to have a serious injury (OR 0.7), and bicyclists 60-69 years (OR 1.5) and 70 and up (OR 1.7) were more likely to have a serious or fatal injury.

### Urban Area Model
Much of the focus of crash countermeasures, including in the NTSB bicycling study, has been on installing bicycle-specific infrastructure in urban areas. Two-thirds (66%) of the BMVCs in the state data took place in urban areas with a posted speed limit of 30 mph or less. I created a separate model for this subset of the statewide crashes. The features included were identical, except that I necessarily excluded urban/rural and speed limit, which do not vary in this subset of the data.

**Model 2: Urban Areas with Posted Speed Limit <= 30 mph**
![Model 2: Urban Areas with PSL <= 30 mph](/images/ORs_urban30.png)

These are the differences compared to the first model:

Location Characteristics

* The midblock OR decreased slightly, from 1.5 to 1.4

Vehicle Type
* The bus OR increased from 2.4 to 3.1.
* The small truck / intersection OR increased from 1.8 to 1.9.
* The large truck / intersection OR increased from 5.3 to 6.6.

Lighting Conditions

* The dark conditions OR decreased from 1.5 to 1.2.
* The dark and unlit conditions OR decreased from 1.8 to 1.7.
* The dawn crashes were no longer different from daylight.

Collision Type

* sideswipe (opposite direction) collisions  had a newly significant OR of 0.5
* rear-end collisions were no longer different from angle collisions in severity
* the OR for head-on collisions decreased slightly, from 1.5 to 1.4

Operator Behavior
* The OR for alcohol-related crashes increased from 2.3 to 2.7.
* The OR for speeding-related crashes decreased from 2.4 to 1.8.
* The OR for drug-related crashes decreased from 3.4 to 2.1.
* Bicyclists helmet use had no significant effect.
* Hit and run crashes had no significant effect.


Female and elderly bicyclists no longer had a statistically different odds of injury compared to male bicyclists.

### Discussion of Model Results
The injury severity model lets us estimate the independent effect of various features controlling for the others in the model. The results make clear that the odds of a serious injury (given that a crash has occurred) increase in rural areas compared to urban ones and with increasing posted speed limit. A midblock crash location also increases severity. Crashes in hours of darkness, particularly on unlit roads and a dawn, increase the severity risk. Crashes that involve buses and trucks are more likely to be severe, particularly in the case of large truck crashes at intersections. Driver impairment also has a very large effect on severity.

Although not measurable from this data, it is extremely likely that driver impairment and darkness not only increase the risk of injury but increase the risk of a crash occurring.

We also have evidence about the effect of the manner of collision on injury severity. Head-on and rear-end collisions are more likely to be serious, but rear-end collisions only marginally so. Sideswipe collisions (where the bicyclist and motorist are traveling in the same direction) are *less* likely to be serious or fatal compared to angle collisions, the most common type.

The model using crashes on urban low-speed roadways differed in a few important ways. In urban areas, rear-end collisions are not likely to be more serious than angle collisions. Driver speeding, driver drug use, darkness are somewhat less strong predictors of severity, but still among the most important. Driver alcohol use was became a somewhat stronger predictor of severity. The most important predictor was vehicle type, and this strengthened for all categories--buses, small trucks, and large trucks. In the case of trucks, the increased risk involves intersection crashes.

I compared these results with the injury severity in the NTSB bicycle report, as shown in the table below. The rural effect was virtually identical. However, the effect of a midblock crash location (compared to intersection or driveway) was lower, as was the effect of higher speed limits, most dramatically for the highest speed categories. One possible reason for these differences is the many other factors included in my model but omitted from the NTSB model. However, when I re-estimated the model using only these features, the ORs for the speed limit categories increased slightly (to 1.5, 2.2, and 2.2 -- still well below the NTSB ORs), the rural OR was unchanged, and the midblock OR decreased to 1.3. Therefore the omitted variables in the NTSB model are not the main reason that the results are different.

**Comparison with NTSB Model**
![Comparison with NTSB model odds ratios](/images/NTSB_comparison.png)

The NTSB model used 2017 data from four states: Pennsylvania, Minnesota, Texas, and Washington. NTSB reported a total of 6,661 BMVCs in those four states for that year. My data show that Pennsylvania accounted for 1,150 (17%) and Texas for 3,345 (51%). Therefore the NTSB data are skewed towards Texas, and the distribution of BMVCs by speed limit is very different in Texas than Pennsylvania, as shown in the following chart. The speed limits on streets used by Texas bicyclists are much higher, which probably accounts for the difference in the speed effects in the NTSB model compared to the current one.


![Comparison with NTSB model odds ratios](/images/Texas_comparison.png)


### Conclusions
Unlike most other U.S. crash data sources, the Pennsylvania database provides information about the manner of collision for bicycle-motor vehicle collisions (BMVCs). Nearly 70% of BMVCs are angle collisions, and most of these occur at intersections or driveways -- where most turning and crossing movements happen. Rear-end collisions account for only 8% of the total. These figures show that addressing the factors that cause angle collisions would have by far the biggest impact on reducing BMVCs.

Unfortunately, the Pennsylvania data lacks key information about bicyclist behavior that could help us determine the causes of angle collisions: specifically, bicyclist position and direction prior to the crash--on the roadway or sidewalk, and with or against traffic. Nor does this dataset tell us which party violated right-of-way rules. I will address these topics separately using other state databases.

The NTSB report focuses primarily on roadway design as a potential countermeasure, and emphasizes that midblock collisions are more likely to result in serious injuries than intersection collisions, with the assumption that most midblock collisions could be addressed by creating physical barriers on roads. However, the Penn. data show, 41% of midblock collisions are actually angle collisions, not the rear-end and sideswipe collisions that are the target of the NTSB-proposed separated bike lane countermeasure. Another 11% involve wrong-way operators.  

Although my injury severity model confirms the NTSB finding that midblock collisions are more likely to be serious, the risk was only 1.5 times greater rather than 2 times greater in the NTSB model. Moreover, I evaluated the specific collision types (rear-end and sideswipe) that could be affected by the separated bike lanes proposed by the NTSB. Rear-end collisions are somewhat more likely to result in serious or fatal injuries compared to angle collisions (OR of 1.2), but not on low-speed urban roads, which is where most separated bike lanes have been installed. Sideswipe collisions are significantly less likely to be serious than angle collisions. On the other hand, head-on collisions were the most likely to be serious or fatal.

I found that the injury severity risk increases with increasing posted speed limits, although not as sharply as in the NTSB model. This discrepancy is likely explained by the high share of Texas crashes in the NTSB data, and the much higher posted speeds on Texas roads where bicycle crashes occur. There are several options to reduce bicycling risk under such conditions, including road diets and other redesigns intended to slow traffic and providing alternate, connected routes through lower-speed streets. As noted by the NTSB, many urban areas have recently reduced default speed limits.

I found several other factors that were omitted from the NTSB model that were as or more important than midblock location in determining injury severity: truck or bus involvement, especially at intersections; motorist impairment; and darkness. All of these factors might be addressed by non-infrastructure measures such as: making bicyclists aware of the importance of staying behind, not beside,  trucks and buses, especially at intersections; reducing the incidence of drunk and drugged driving and speeding; and increasing the use of lights on bicycles in hours of darkness. Reducing wrong-way bicycling could reduce head-on collisions, the most severe type. All of these measures are almost certain to reduce crash frequency, in addition to reducing crash severity.
