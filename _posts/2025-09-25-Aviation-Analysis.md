---
title: Identifying New Route(s) and Redeploying Resources for KK Airlines
image: "/assets/Post/Aviation1.jpg"
date: 2025-09-25
categories: [Portfolio]
tags: [UiPath, Tableau, Data Visualisation, Data Wrangling, Aviation]
---

## Background

The KK Airlines is a new airline recently established in Singapore that focuses mainly on commercial flights. To gain a stronger presence and foothold in the air transport sector, the management of KK Airlines is keen to explore new international route(s) that are most popular among travellers departing from Singapore Changi Airport. As KK Airlines is new, airplane/crew resource management are also a key consideration of the management. Therefore, they are also keen to find out route(s) that were least visited/popular so that resources could be redeployed to service popular route(s).

Currently, the team of analysts have to manually download the data from data.gov.sg to perform analysis, which is time consuming, reactive and prone to human error.

**Dataset Utilised**

To address management’s query, data shall be extracted from the Total Air Passenger Departures by Country uploaded and maintained by the Civil Aviation Authority of Singapore (i.e. CAAS) through data.gov.sg. The analyst team believes that utilising official data for analysis forms a credible argument in the results generated. 

**Time Period**

Team shall extract data from January 1961 till Feb 2020 (latest data provided by CAAS). For this analysis, team shall only analyse data between the year 2000 to 2020 to enhance the reliability and relevancy of travelling trends.

## Import data into UiPath 

Within the sequence, a “Read Range Workbook” activity is chosen, with the Excel workbook pathway defined in the first box. Under the second box, the tab name is defined with the range left as blank for UiPath to read all cells within. The “Output Datatable” is also created as “dt_Exceldata” – this created variable shall be the defined location where the uploaded Excel will be housed after UiPath completes the reading activity. Lastly, the “Message Box” is included that notify user of the completed loading "Successfully Loaded".

![Picture 1a](assets/Aviation/Picture1a.png)
![Picture 1b](assets/Aviation/Picture1b.png)

## Prepare Datatset for Analysis using Excel

The dataset is downloaded from Total Air Passenger Departures by Country maintained by CAAS. After download, the file is stored in a csv format, therefore, a conversion to .xlsx type file is performed and the dataset is presented below.

![Picture 2a](assets/Aviation/Picture2a.png)

The following columns shall be retitled for better clarity:

* Update “month” to “Month” (keeping first letter capitalised for consistency)
* Update “Level_1” to “Description”
* Update “level_2” to “Region”
* Update “level_3" to "Country”
* Update “value” to “Passenger”

Next, formatting of data were performed within the following columns:

* Under “month” column, the data type shall be updated to “short-date” format instead of “general”.
* Under “Passenger” column, “na” records were present in the dataset. Therefore, “find and replace” function is selected to replace all “na” records to “0”. Once completed, the records are updated to “Number” format with commas and no decimal point instead of “general”.
* The formatting of “Description”, “Region” and “Country” columns are kept at default (“General”).

After all formatting is completed, the “Freeze top row” pane function was applied to perform a sanity check, ensuring no records were left out in the data preparation process. The screenshot below shows the finalised dataset post-cleaning.

![Picture 2b](assets/Aviation/Picture2b.png)

## Creating a Project Management Schedule and Automation Solution

**Creating a Project Management Schedule using Asana**

Before the commencement of this automation, project, the analyst team have drawn up an online Gantt chart using Asana. In the first three weeks of the project, team shall huddle and strategise on the type of data required that are publicly available. At the start of fourth week, team shall book management’s time to pitch on the data strategy adopted, take in any further comments from management before commencing on the next phase of the project after approval is sought.

![Picture 3a](assets/Aviation/Picture3a.png)
![Picture 3b](assets/Aviation/Picture3b.png)

Once approval on the data approach is sought, team shall commence the onboarding of UiPath technical team (external vendor brought in by the analyst team to oversee automation project and to provide hyper care assistance). Concurrently, the team commences with the automation project and to seek help from UiPath technical team when required. On the last Friday where team is building the automation which includes (a) building of UiPath Sequence, (b) Performing User Acceptance Test (UAT) and (c) Refining details from UAT result. In addition, an update will be provided to management to update them on the progress of the project during this phase, as well as to share if any issues were encountered along the way.

![Picture 3c](assets/Aviation/Picture3c.png)

Once all the preparation and setup work is completed, the team shall officially commence on their automation project. In this phase, the team will not be updating the management on the progress (assumed is agreed upon previously) and to focus on the analysis. The UiPath technical team is still around to provide support should the team requires it. After three weeks of preparation, the analyst team will present their final results gathered for management‘s attention. After the presentation is concluded, the UiPath team will be offboarded and the project is considered complete.

![Picture 3d](assets/Aviation/Picture3d.png)
![Picture 3e](assets/Aviation/Picture3e.png)

## Automation Solution using UiPath

First, the “Use Application/Browser” activity is selected for the purpose of automatic download. Next, under the “do” sequence, the “click” activity is selected with an on-screen instruction on the area where I require UiPath to access – in this case, it is the “Download CSV (541.9kb)” button. The remaining properties were left at its default. The file will be automatically saved in the “downloads” folder.

![Picture 4a](assets/Aviation/Picture4a.png)
![Picture 4b](assets/Aviation/Picture4b.png)

Once completed, the “Excel Process Scope” activity is included in the next sequence. Under the “Do” sequence, the “Use Excel File” is included to instruct UiPath of my intention to utilise the downloaded data. The remaining properties were left as default.

![Picture 4c](assets/Aviation/Picture4c.png)

Next, within the second “Do” sequence, the “Read Range Workbook” activity is included. The sheet is referenced to the downloaded dataset earlier while the sheet is defined as “TotalAirPassengerDeparturesbyCo”, which is the stored sheet name in the dataset. The rage is set as empty “” as we would want UiPath to read all records within the dataset. The important note here is an “Output DataTable” is created named “DT” to store all records within UiPath first before transferring to Goggle Sheet for analysis.

![Picture 4d](assets/Aviation/Picture4d.png)

Next, the “Write Range” activity for Google Sheets is included as the next sequence Refer to Screenshot 4h for the details included in the fields. The important note here is that the blank Aviation spreadsheet is created beforehand to house all records within. In addition, a new project is created on Google Could for the purpose of activating both Google Drive and Google Sheet’s API, with the access to both documents granted to the service account.

![Picture 4e](assets/Aviation/Picture4e.png)
![Picture 4f](assets/Aviation/Picture4f.png)
![Picture 4g](assets/Aviation/Picture4g.png)

Once all setup and activities are completed, the process run was performed by clicking “Debug File”. We can see that the Output message showed in UiPath has successfully completed its task with the records transferred to the Aviation Google Sheet.

![Picture 4h](assets/Aviation/Picture4h.png)
![Picture 4i](assets/Aviation/Picture4i.png)

As part of the final automation process, a connection has been established between the Aviation Google Sheet and Tableau interface. Therefore, whenever the team hits the “Run File” button in UiPath, the latest dataset will be stored in UiPath and transferred to Aviation Google Sheets, which in turn connects to the Tableau server. This ensures that the team will always have latest data records to perform analysis while improving the accuracy of analysed result that will assist the management of KK Airline in making an informed decision.

![Picture 4j](assets/Aviation/Picture4j.png)
![Picture 4k](assets/Aviation/Picture4k.png)

## Data Storytelling and Visualisation via Tableau

The analysis considered data between January 2000 to February 2020 (latest data provided by CAAS) in enhancing its relevance and reliability of the results generated. Please refer to the embedded Tableau Public link for the interactive [Tableau Storyboard](https://public.tableau.com/app/profile/kaikiat/viz/AnalysisofRoutesforKKAirlines/Story2).

*Story 1: Popularity by Regions*
![Picture 5a](assets/Aviation/Picture5a.png)

From the first storyboard, it is evident that the “Southeast Asia” region (i.e. SEA) cements its dominance as the most popular region by travellers disembarking from Singapore Changi Airport for the past 20 years (i.e. between 2000 to 2020). The total passenger movement is almost twice of Northeast Asia (~169M vs ~86M). SEA’s popularity could be explained by the diverse experiences, rich culture heritage and lower cost of living it offers to travellers. Therefore, the team is confident about establishing new routes within the SEA region.

*Story 2: Popularity by Country*
![Picture 5b](assets/Aviation/Picture5b.png)

Analysing the popularity by country, the top three choices were evident as they were all from the SEA region, followed by Northeast Asia (i.e. NEA) and Europe/UK. Surprisingly, Vietnam was ranked as the third least popular country to visit by travellers, a stark contrast relative to its SEA counterparts. Vietnam’s low popularity could be explained by the possession of fewer tourist hubs, infrastructure gaps and the lack of locals speaking the English language outside of major cities (e.g. Ho Chi Minh, Hanoi).

*Story 3: Analysis of Top/Bottom 3 Countries*
![Picture 5c](assets/Aviation/Picture5c.png)

Diving deeper into the top/bottom 3 countries by its month (i.e. seasonality), there is a distinct pattern. Number of travellers visiting the SEA regions (Indonesia, Malaysia, Thailand and Vietnam) showed bullish movements in December whereas the demand for Europe (France and Germany) remained sluggish. Within the Singapore context, December marks the school holiday for children studying in institutions below tertiary level. Coupled with festive celebrations and public holidays such as Christmas and New Year, families and individual travellers alike would be inclined to maximise their travel plans as much as possible. Furthermore, as SEA countries are relatively nearby in terms of its geographical location, it is feasible for travellers to visit a few of SEA countries in a single trip (i.e. The flight from Kuala Lumpur International Airport in Malaysia to Suvarnabhumi Airport in Bangkok, Thailand only takes about 2 hours of flight time).

To maximise revenue, team proposes to reassign planes and flight crew that serves France and Germany routes to Indonesia, Malaysia and Thailand instead. This proposal doubles up as an effective resource management technique as it would be less resource intensive to only focus on SEA for a start. To further capitalise on travel demand on these countries, management could then consider setting up base stations in the top 3 SEA countries to better serve the needs of travellers, further expanding its foothold and influence in the air transport sector. In the longer run, team proposes to drop airline routes to Europe, especially France and Germany as demand has remained relatively low across 20 years of records. As the routes to focus are of shorter distance to Singapore, management could also consider selling/ending leases early for larger aircraft models (e.g. Airbus 350,380) in favour of smaller aircraft. This would also help KK Airlines reduce fixed costs in maintaining its fleet of planes, ensuring that the business remains sustainable and affordable for all.

Furthermore, as Indonesia is the most popular country to visit, management could consider reassigning airplanes and flight crew to serve this route in the month of July. To further enhance its brand awareness and demand, management could consider providing discounted fares and/or establish partnerships with the airport operator (i.e. Changi Airport Group) to roll out marketing campaigns.

*Storyboard 4: Bonus Analysis of Northeast Asia*
![Picture 5d](assets/Aviation/Picture5d.png)

Looking into Northeast Asia routes, team has identified further opportunities for management’s consideration. For instance, management may consider increasing the frequency of trips in August for China, Hong Kong and Japan to capture the surge in travel demand. Moreover, additional flights could be catered to Japan for the month of March as it coincides with the Sakura blooming season and international events such as the Tokyo Marathon.

## Summary

To recap, the team recommends the following strategies for management’s consideration:

1. To focus on SEA routes (especially Indonesia, Malaysia and Thailand) in maximising profits through capturing market demand;
2. Expand number of trips in December for SEA routes and to Indonesia in July;
3. To roll out marketing campaigns for Indonesia in July to boost airline branding;
4. Set up base stations in Indonesia, Malaysia and Thailand to better serve the needs of travellers;
5. Consider increasing flight frequency for NEA (i.e. China, Hong Kong and Japan) in August; and
6. To also offer increased trips for Japan in March as it coincides with the Sakura blooming season and international events such as the Tokyo Marathon; and
7. In the longer term, to consider selling/ending leases early for larger aircraft in favour of smaller ones to reduce fixed cost.