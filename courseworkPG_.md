---
id: litvis
narrative-schemas:
    - ../narrative-schemas/courseworkPG.yml
elm:
    dependencies: {gicentre/elm-vegalite: latest, gicentre/tidy: latest}
---  
  
  
<style>@import url("https://fonts.googleapis.com/css?family=Roboto+Condensed:300|Roboto+Slab|Fjalla+One|Caveat");
#litvis {
  font-family: "Roboto Condensed", sans-serif;
  font-size: 11pt;
}
#litvis h1,
#litvis h2,
#litvis h3,
#litvis h4,
#litvis h5,
#litvis h6 {
  font-family: "Fjalla One", sans-serif;
}
#litvis h2 {
  font-size: 19.8pt;
}
#litvis h3 {
  font-size: 13.2pt;
}
#litvis h4,
#litvis h5,
#litvis h6 {
  font-size: 11pt;
}
#litvis blockquote {
  font-family: "Roboto Slab", serif;
  font-size: 12.1pt;
  background-color: #f6f6f6;
  margin-right: 2em;
}
#litvis blockquote h1,
#litvis blockquote h2,
#litvis blockquote h3,
#litvis blockquote h4,
#litvis blockquote h5,
#litvis blockquote h6 {
  font-family: "Roboto Slab", sans-serif;
}
#litvis code,
#litvis pre {
  background-color: rgba(255, 255, 255, 0);
}
#litvis pre {
  border: solid 1px #ccc;
  border-radius: 8px;
}
#litvis div[data-role="litvisOutput"] > span {
  font-family: iosevka, monospace;
  font-weight: bold;
  font-size: 17.6pt;
  display: block;
  margin-bottom: 0.75em;
  border-style: none none none solid;
  border-width: 1px;
  border-color: #ccc;
  padding-left: 0.5em;
}
</style>
  
```elm
import Tidy exposing (..)
import VegaLite exposing (..)
```
  
# Postgraduate Coursework Template
  
  
{(questions|}
  
Health care costs in the United States are some of the most expensive among developed countries. Many hospitals and insurance providers negotiate prices independently, resulting in large variations in charges for the same procedure at different healthcare centers within the same city. I would like to explore these charges to see if medical charges are truly random, or if there exist any patterns or geographic trends.
  
- Do charges for the same procedure vary consistently across regions (cities, states, geographic regions) or are there clusters of higher/lower variance in pricing?
  
{|questions)}
  
## Data
  
  
The Centers for Medicare and Medicaid Services (CMS) provides public records of [Inpatient and Outpatient Charge Data](https://data.cms.gov/Medicare-Inpatient/Inpatient-Prospective-Payment-System-IPPS-Provider/fm2n-hjj6 ) from health care providers from every state between 2011 - 2016. These files contain information about the DRG (Diagnosis Related Group), health care provider name, city, zip code, hospital referral region (HRR), and the number of discharges (cases), Average Charge, Average Medicare Payment received, and Averaged Total Payment received. To narrow the scope of this project, I used the 2015 data and considered only the Average Charges for a few of the most common diagnoses:
  
- Simple Pneumonia
- Kidney and Urinary Tract Infections
- Septicemia
- Heart Shock and Failures
  
{(visualization|}
  
I created four charts, one for each of the most common diagnoses.
  
```elm
goldenRatio : Float
goldenRatio =
    1.618
```
  
```elm
pneumonia : Spec
pneumonia =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
  
        data =
            dataFromUrl "./pneumonia_2015_regions_ranks.csv"
  
        enc =
            encoding
                << position X
                    [ pName "State"
                    , pMType Nominal
                    , pAxis [ axLabelAngle 45, axDomain False ]
                    , pScale [ scDomain (doStrs divOrder) ]
                    ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative, pTitle "Average Charge" ]
                << color
                    [ mName "Divison"
                    , mMType Nominal
                    , mScale [ scScheme "tableau10" [] ]
                    , mLegend
                        [ leOrient loTopRight
                        , leFillColor "rgb(255,255,255)"
                        , leStrokeColor "rgb(220,220,220)"
                        , leRowPadding 0.7
                        , leColumnPadding 0.9
                        , lePadding 10
                        , leTitleFontSize 15
                        ]
                    ]
    in
    toVegaLite [ width 700, height (700 / goldenRatio), title "Price Variation: Pneumonia  2015", data [], boxplot [ maSize 10, maOpacity 0.7 ], enc [] ]
```
  
```elm
kidney : Spec
kidney =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
  
        data =
            dataFromUrl "./kidney_uti_2015_regions_ranks.csv"
  
        enc =
            encoding
                << position X
                    [ pName "State"
                    , pMType Nominal
                    , pAxis [ axLabelAngle 45 ]
                    , pScale [ scDomain (doStrs divOrder) ]
                    ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative, pTitle "Average Charge" ]
                << color
                    [ mName "Divison"
                    , mMType Nominal
                    , mScale [ scScheme "tableau10" [] ]
                    , mLegend
                        [ leOrient loTopRight
                        , leFillColor "rgb(255,255,255)"
                        , leStrokeColor "rgb(220,220,220)"
                        , leRowPadding 0.7
                        , leColumnPadding 0.9
                        , lePadding 10
                        , leTitleFontSize 15
                        ]
                    ]
    in
    toVegaLite [ width 700, height (700 / goldenRatio), title "Price Variation: Kidney & Urinary Tract Infections 2015", data [], boxplot [ maSize 10, maOpacity 0.7 ], enc [] ]
```
  
```elm
septicemia : Spec
septicemia =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
  
        data =
            dataFromUrl "./septicemia_2015_regions_ranks.csv"
  
        enc =
            encoding
                << position X
                    [ pName "State"
                    , pMType Nominal
                    , pAxis [ axLabelAngle 45 ]
                    , pScale [ scDomain (doStrs divOrder) ]
                    ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative, pTitle "Average Charge" ]
                << color
                    [ mName "Divison"
                    , mMType Nominal
                    , mScale [ scScheme "tableau10" [] ]
                    , mLegend
                        [ leOrient loTopRight
                        , leFillColor "rgb(255,255,255)"
                        , leStrokeColor "rgb(220,220,220)"
                        , leRowPadding 0.7
                        , leColumnPadding 0.9
                        , lePadding 10
                        , leTitleFontSize 15
                        ]
                    ]
    in
    toVegaLite [ width 700, height (700 / goldenRatio), title "Price Variation: Septicemia 2015", data [], boxplot [ maSize 10, maOpacity 0.7 ], enc [] ]
```
  
```elm
heartshock : Spec
heartshock =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
  
        data =
            dataFromUrl "./heart_shock_failure_2015_regions_ranks.csv"
  
        enc =
            encoding
                << position X
                    [ pName "State"
                    , pMType Nominal
                    , pAxis [ axLabelAngle 45 ]
                    , pScale [ scDomain (doStrs divOrder) ]
                    ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative, pTitle "Average Charge" ]
                << color
                    [ mName "Divison"
                    , mMType Nominal
                    , mScale [ scScheme "tableau10" [] ]
                    , mLegend
                        [ leOrient loTopRight
                        , leFillColor "rgb(255,255,255)"
                        , leStrokeColor "rgb(220,220,220)"
                        , leRowPadding 0.7
                        , leColumnPadding 0.9
                        , lePadding 10
                        , leTitleFontSize 15
                        ]
                    ]
    in
    toVegaLite [ width 700, height (700 / goldenRatio), title "Price Variation: Heart Shock and Failure 2015", data [], boxplot [ maSize 10, maOpacity 0.7 ], enc [] ]
```
  
{|visualization)}
  
{(insights|}
  
Across the four diagnoses, the states of New Jersey, Nevada, California, and Florida consistently have the highest charges, followed by Arizona, Texas, Alaska, and Colorado. Pennsylvania and New York consistently have a large IQR range. Excluding the most expensive states, the average charge for Pneumonia and Heart Shock and Failure treatments ranges between \$15,000 and \$35,000, between \$20,000 and \$60,000 for Septicemia, and between \$10,000 and \$30,000 for Kidney and Urinary Tract Infections. This proves that significant variation in pricing exists in every state, however, few discernable patterns related to geography are evident. In the case of New Jersey, California, and Florida, the higher charges could be potentially be explained by their large populations. This suggests that geographic region may not be strongly correlated with pricing and thus a poor explanatory factor for the variation in charges, but more statistical analysis is needed to support this hypothesis.
  
{|insights)}
  
{(designJustification|}
  
**Visual Variable**
  
I wanted to avoid using an average to summarize the charges in a state since they can hide variation. With averages, one state with several low-cost procedures and one high-cost procedure could appear similar to another state with just a few medium cost procedures. A box-and-whisker plot is a quantitative visual variable shows the range in which the majority of values lie, the range between the min and max value, and any outliers. I felt that this would be a more accurate representation of aggregated data for each state. Additionally, box plots facilitate additional comparisons of the spread of values, and number and extent of outliers when juxtaposed against each other.
  
**Color**
  
The United States can be divided into four major statistical regions: the Northeast, the Midwest, the South, and the West, and nine underlying geographic regions: New England, Mid-Atlantic, East North Central, West North Central, South Atlantic, East South Central, West South Central, Mountain and Pacific (U.S. Census Bureau, 2013). The box plots, however, represent charges that are aggregated by state. Munzner (2015) suggests that spatial location and hue are the most effective channels for representing categorical attributes. Following this logic, I used the color of a box plot to indicate what region the state belongs to. I also placed box plots of states in the same region next to each other on the graph. I used the Tableau10 distinct color scheme since the regions are categorical values and added a legend as a guide. This way, I am able to create visual groupings to support analysis. I also set the opacity to 0.7 for a softer look.
  
**Orientation**
  
I changed the orientation of the state labels to be angled at 45 degrees for ease of reading. Generally, horizontal text is preferable to vertical text which can difficult to read (Tufte, 2001). In the default vertical orientation, state names must be read from bottom to top, which may involve turning one's head and scanning along a jagged edge of text since the only the ends of the words were aligned with the axis. While it was not possible to orient the text horizontally due to space constraints, the 45-degree orientation aligned the start of the state name with the x-axis, hopefully making it easier to scan across and read.
  
I also had to decide which on which axis to have the box plots. Designer Ann Emery (2017) suggests that sorted, horizontal bar charts are better for the comparison of categorical values, and vertical column bar charts are better for ordinal values since they can be ordered by their natural progression. Even though the states are categorical values, I instead chose to have the states on the horizontal axis resulting in vertical box plots. This made it easier to compare the box plots against the qualitative axis ( Average Charge) when it appears on the left instead of at the very bottom of the chart.
  
**Layout**
  
A majority of the box plot data is located at the bottom of the chart. The presence of outliers, however, increased the upper bound of the qualitative scale. As as a result, I had a great deal more whitespace that I was comfortable with at the top of the chart. To utilize this space and create a more compact design, I moved to the legend inside the boundaries of the chart. To make it distinct from the rest of the chart, I kept the white background and added a border outline to the box.
  
{|designJustification)}
  
{(validation|}
  
**Strenths**
I believe this visualisation is effective in depicting regional variation in charge data among states and regions. The use of a box plot as a visual variable directly aids in visualising and comparing those variations. Secondly, the use of color and colocation for grouping is an effective means of creating distinct categories.
  
**Limitations**
First of all, I am still dissatisfied with the level of clutter on the visualisation. The outliers, while interesting artefacts, perhaps serve to create more "chart junk" -- as termed by Tufte (2001) -- than do serve as explanatory factors. In order to improve my data-ink ratio, I would consider removing the outliers, as well as the horizontal grid lines.
  
Second, the state names are still rather small and difficult to read. This is not easy to resolve, however, as increasing the font size causes the labels to overlap and increasing the space between box plots to allow for a bigger label font results in a bigger graph. I was thinking of allowing the user to interactively toggle of state names by region to reduce clutter.
  
Third, I feel having nine regional categories, and thus nine different colors, adds unneeded complexity to the chart. In their study of the limits of human attention, Haroz and Whitney (2012) recommend fewer categories in visualisations to avoid overwhelming the user. Following this advice, I would go back to a design that has only 4 or 5 regions to simplify the visualisation.
  
Finally, this design doesn't support effective comparison between different DRG types (i.e. Pneumonia versus Septicemia). Currently, the four charts are arranged vertically, and the y-axes are scaled independently. This kind of juxtaposition requires the user to scroll to see each chart and remember important differences. As a result, the user can fail to notice certain details like the fact that Septicemia has much more expensive charges than do the other three DRGs. I would question if it was really necessary to compare different DRGs against one another since the research question is focused on variations between geographic regions. If such a comparison is still desired, a butterfly chart may a more effective tool for comparison.
  
**Futher Explorations**
  
With this visualisation, I have only looked at a small aspect of the available data. The variation in Medicare reimbursements and customer payments could be visualized in a similar manner. Additionally, I would be interested in exploring temporal trends, perhaps using a stream graph to visualize the variation over time. Finally, to add interaction to the existing plots, I believe it would be nice to have a tooltip that displays the precise min, max, median, average, and first and third quartile values when the mouse moves over a box plot.
  
{|validation)}
  
## Previous Designs
  
  
### Version 1 : Horizontal Boxplot Chart
  
  
In this first attempt, I compared the variation in charges across the 306 hospital referral regions (HRR) using box plots and used color to group HRR regions by state. While visually interesting, the chart was just too long to be practical. This gave me the idea of trying to arrange the box plots geospatially in a relaxed-spatial grid of the United States as seen in Version 2. (Scroll past this chart)
  
```elm
histo : Spec
histo =
    let
        data =
            dataFromUrl "./pneumonia_2015_rowcol.csv"
  
        enc =
            encoding
                << position Y [ pName "Hospital Referral Region (HRR) Description", pMType Nominal ]
                << position X [ pName "Average Covered Charges", pMType Quantitative ]
                << color [ mName "HRR State", mMType Nominal, mScale [ scScheme "tableau10" [] ], mLegend [ leTitle "State" ] ]
    in
    toVegaLite [ width 600, title "Variation of Pneumonia Charges 2015", data [], boxplot [ maSize 9 ], enc [] ]
```
  
### Version 2 : Relaxed Grid Layout
  
  
Based on the idea of using small multiples of a plot in a grid layout that only loosely approximates geographical regions (Meulemans et al., 2017), I created an interactive grid version of the United States. Unfortunately, I was unable to get this relaxed-geographically grid layout to display properly. I believe it is in part due to the assumption in Vega that each subplot has the same axis values. So even though each state has only a subset of the data, the y-axis was still scaled for 306 values, resulting in boxplots being squished together. Not only was the size of this graphic a problem, but it would have been difficult to embed the necessary labels without creating even more visual clutter.
  
```elm
dwellingMap : Spec
dwellingMap =
    let
        data =
            dataFromUrl "./pneumonia_2015_rowcol.csv"
  
        enc =
            encoding
                << position Y [ pName "Hospital Referral Region (HRR) Description", pMType Ordinal, pAxis [] ]
                << position X [ pName "Average Covered Charges", pMType Quantitative, pAxis [] ]
                << row [ fName "Row", fMType Ordinal, fHeader [ hdTitle "" ] ]
                << column [ fName "Column", fMType Ordinal, fHeader [ hdTitle "" ] ]
                << tooltip [ tName "Hospital Referral Region (HRR) Description", tMType Nominal ]
  
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration (coFacet [ facoSpacing 0 ])
    in
    toVegaLite [ width 50, data [], cfg [], width 80, height 100, boxplot [], enc [] ]
```
  
### Version 3: Vertical Column plots
  
  
Instead of 306 HRR regions, I finally chose to aggregate data by the 50 U.S. states (and Washington D.C.) for 51 categorical values. States were grouped by color into super-regions (Midwest, West, South, etc). I also made the move to a vertical column plot so that all the data is visible at once. While I like the visual simplicity of four major regions, I was concerned that four was too few and could lead to inappropriate comparisons between dissimilar states. Therefore, in the next iteration, I chose to use nine geographic subregions ( Pacific, Mountain, Northeast, etc).
  
```elm
pneuFour : Spec
pneuFour =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
  
        data =
            dataFromUrl "./pneumonia_2015_regions_ranks.csv"
  
        enc =
            encoding
                << position X
                    [ pName "State"
                    , pMType Nominal
                    , pAxis [ axLabelAngle 45, axDomain False ]
                    , pScale [ scDomain (doStrs regionOrder) ]
                    ]
                << position Y [ pName "Average Covered Charges", pMType Quantitative ]
                << tooltips
                    [ [ tName "State", tMType Nominal ]
                    ]
                << color
                    [ mName "Region"
                    , mMType Nominal
                    , mScale [ scScheme "tableau10" [] ]
                    , mLegend
                        [ leOrient loTopRight
                        , leFillColor "rgb(255,255,255)"
                        , leStrokeColor "rgb(220,220,220)"
                        , leRowPadding 0.7
                        , leColumnPadding 0.9
                        , lePadding 10
                        , leLabelFontSize 15
                        , leTitleFontSize 16
                        ]
                    ]
    in
    toVegaLite [ width 700, height (700 / goldenRatio), title "Cost Variation: Pneumonia 2015", data [], boxplot [ maSize 10, maOpacity 0.7 ], enc [] ]
```
  
{(references|}
  
**Ann K.Emery** (2017) When to Use Horizontal Bar Charts vs. Vertical Column Charts [Online]. _Depict Data Studio_. URL http://depictdatastudio.com/when-to-use-horizontal-bar-charts-vs-vertical-column-charts/ (accessed 5.5.19).
  
**Haroz, S., and Whitney, D.** (2012) How capacity limits of attention influence information visualization effectiveness. _IEEE Transactions on Visualization and Computer Graphics_, 18(12) pp.2402-2410.
  
**Meulemans, W., Dykes, J., Slingsby, A., Turkay, C. and Wood, J.** (2017) Small multiples with gaps. _IEEE Transactions on Visualization and Computer Graphics_, 23(1), pp.381-390.
  
**Munzner, T.** (2014) Visualization Analysis and Design. CRC Press LLC, Florida, UNITED STATES.
  
**Tufte, E.** (2001) The Visual Display of Quantitative Information, Graphics Press.
  
**United States Census Bureau, Geography Division.** (2013) "Census Regions and Divisions of the United States" (PDF). Retrieved May 04, 2019.
  
{|references)}
  
```elm
regionOrder : List String
regionOrder =
    [ "Missouri"
    , "Illinois"
    , "Wisconsin"
    , "Indiana"
    , "Iowa"
    , "Ohio"
    , "Nebraska"
    , "South Dakota"
    , "Kansas"
    , "Michigan"
    , "Minnesota"
    , "North Dakota"
    , "Connecticut"
    , "Maine"
    , "Massachusetts"
    , "Rhode Island"
    , "New York"
    , "New Hampshire"
    , "New Jersey"
    , "Pennsylvania"
    , "Vermont"
    , "Alabama"
    , "Georgia"
    , "Florida"
    , "Mississippi"
    , "Arkansas"
    , "Tennessee"
    , "Delware"
    , "Maryland"
    , "DistrictofColumbia"
    , "Kentucky"
    , "Oklahoma"
    , "West Virginia"
    , "Louisiana"
    , "Texas"
    , "North Carolina"
    , "Virginina"
    , "South Carolina"
    , "Alaska"
    , "Arizona"
    , "New Mexico"
    , "Nevada"
    , "California"
    , "Oregon"
    , "Colorado"
    , "Hawaii"
    , "Idaho"
    , "Washington"
    , "Montana"
    , "Utah"
    , "Wyoming"
    ]
```
  
```elm
divOrder : List String
divOrder =
    [ "New York"
    , "New Jersey"
    , "Pennsylvania"
    , "Illinois"
    , "Wisconsin"
    , "Indiana"
    , "Ohio"
    , "Michigan"
    , "Missouri"
    , "Iowa"
    , "Nebraska"
    , "South Dakota"
    , "Kansas"
    , "Minnesota"
    , "North Dakota"
    , "Arizona"
    , "New Mexico"
    , "Nevada"
    , "Colorado"
    , "Idaho"
    , "Montana"
    , "Utah"
    , "Wyoming"
    , "Connecticut"
    , "Maine"
    , "Massachusetts"
    , "Rhode Island"
    , "New Hampshire"
    , "Vermont"
    , "Alaska"
    , "California"
    , "Oregon"
    , "Hawaii"
    , "Washington"
    , "Alabama"
    , "Mississippi"
    , "Tennessee"
    , "Kentucky"
    , "Arkansas"
    , "Oklahoma"
    , "Louisiana"
    , "Texas"
    , "Georgia"
    , "Florida"
    , "Delware"
    , "Maryland"
    , "District of Columbia"
    , "West Virginia"
    , "North Carolina"
    , "Virginina"
    , "South Carolina"
    ]
```
  