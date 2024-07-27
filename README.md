# :computer: Marketing Campaign Data Analysis
In this project, I intend to play a role of data analyst to analyze marketing data of my client, called Maven Marketing. I used **Microsoft Excel** to explore and analyze their marketing campaign data, which contains information on 2,240 customers. I first cleaned the data to ensure integrity and then analyzed it using `PivotTables` and various `Charts`. Finally, I highlighted key findings and delivered the data analysis report using **Tableau** Story.

For the purpose of this analysis, the following general assumptions are made:
1. The data is up-to-date as of 2024.
2. The amounts spent on products are in US dollars.
3. The four sales channels — Web, Catalog, Store, and Deals — are mutually exclusive and do not overlap.

🌼 Dataset Link: [Maven Analytics](https://www.mavenanalytics.io/data-playground?order=date_added%2Cdesc&search=marketing)

The main objectives are to:
- Identify customer segmentation based on their demographics (e.g., age, education, marital status) and the types of products they buy the most (e.g., meat, fish, wines) to better understand customers and target main customers with customized services.
- Evaluate the effectiveness of past marketing campaigns (AcceptedCmp1-5 and Response in the given dataset).
- Evaluate the performance of different sales channels (e.g., web, catalog, store) and identify areas for improvement in the company's online and offline sales strategies.

## 📚 Dataset Description:

![image](https://github.com/user-attachments/assets/b1201bce-8889-44fc-a3c9-9429415ed6ff)


## :bulb: Data Cleaning Process

### 1. Handling Missing Values:
- Identified missing values using the `COUNTBLANK()` or `IF()` function.
- Applied conditional formatting by creating a new rule to highlight blanks, revealing that only the `'Income'` column contained blanks.
- Filtered the `'Income'` column to show only blanks and counted 24 missing entries.
  ![image](https://github.com/user-attachments/assets/26145be5-462d-484b-b1c9-c3eb5a62e71a)
  


  ![image](https://github.com/user-attachments/assets/200180b5-2f24-4331-bf62-31f05b990caa)

- Since the distribution of the `'Income'` column is right-skewed with outliers, the median (51411) was used to replace the 24 missing values (average: 52284.82).

### 2. Outlier Detection:
- Identified 113734 as the 75th percentile in the `'Income'` column shown above.
- Filtered and flagged values above this threshold, resulting in 8 rows of outliers; but were retained for later because it will be grouped into bins.

  
- Next, analyzed the `'Year_Birth'` distribution and found it to be left-skewed.
  ![image](https://github.com/user-attachments/assets/c8a7a807-9008-434a-9e11-c84fa1d3508d)
- Since 1940 is shown as the lower whisker value, the values below it are outliers. Those years of birth were `1893`, `1899`, and `1900`, which imply those customers' ages are at least 113 years at the time of enrollment, which is highly unlikely. Consequently, 3 rows were removed.
  ![image](https://github.com/user-attachments/assets/55fbf303-72e9-4991-b865-d87684e391e0)

### 3. Checking for Duplicates:
- Initial verification with the `'ID'` column, a unique identifier for each user, appeared to have no duplicates.
  ![image](https://github.com/user-attachments/assets/2b61b1e2-6919-4a67-add2-f24f5e2393ee)

- However, to identify potential duplicates with different IDs, concatenated values from all columns (excluding `'ID'`) into a new column and applied conditional formatting. This revealed 47 duplicate pairs, which were then removed using the `"Remove Duplicates"` feature.
  ![image](https://github.com/user-attachments/assets/1947490b-e3a6-4f81-87f0-99e71ba80918)

  ![image](https://github.com/user-attachments/assets/ea4be7b8-1c11-468f-ad3f-f0745c6533ff)

### 4. Cleaning the Marital Status Column:
- Reclassified `'Alone'` as `'Single'`, so 3 entries were updated.
- Deleted `'Absurd'` and `'YOLO'` entries (2 each) as they were not standard marital status categories and their meanings were unclear.
 ![image](https://github.com/user-attachments/assets/30113ce7-b279-4dec-9016-f6f7c2453978)

### 5. Binning Columns:
- **Year of Birth**:
  - Ordered the `'Year_Birth'` column chronologically.
  - Created a new `'Age'` column using the formula: `=YEAR(TODAY()) - D2`
  ![image](https://github.com/user-attachments/assets/32e4f5e8-504d-48f3-bff8-f78d2bf6b18c)

  - Instead of using `Age` directly, I decided to group the age range by generation to better understand cohort similarities. The definition I refer to is [here](https://www.beresfordresearch.com/age-range-by-generation/).
  - Added a `'Generation'` column with the formula:
  ```
  =IF(C2>=12,IF(C2<=27,"Gen Z",IF(C2<=43,"Millennials",IF(C2<=59,"Gen X",IF(C2<=69,"Boomers II",IF(C2<=78,"Boomers I",IF(C2<=96,"Post War",IF(C2<=102,"WWII","Unknown"))))))),"Unknown")

  ```

  ![image](https://github.com/user-attachments/assets/911041fb-e0fb-482f-8a0d-16151611b43a)

- **Income**:
  - Created income bins with $10,000 intervals in the `'Income'` column, which resulted in 11 bins.
  ![image](https://github.com/user-attachments/assets/b414630c-a0a1-4443-8f24-2ac29fc581bd)
  ![image](https://github.com/user-attachments/assets/10537ef3-2b79-401d-80ce-57b751864396)

 
  - However, later, the bins were adjusted to $20,000 intervals to reduce the number of bins.

    ![image](https://github.com/user-attachments/assets/eed3e58d-d85b-458c-bc86-21ac096420b8)

  - Moreover, the `'Above 100000'` bin was combined with the `'80001 – 100000'` bin to form the `'Above 80000'` bin due to the small number of people in the former.

### 6. Created New Columns:
- Created a `'Total accepted camp'` column by summing the acceptance of all six campaigns for each customer.
  <img width="608" alt="image" src="https://github.com/user-attachments/assets/462baed3-2812-4425-a32c-9ea6169c7961">


- Created a `'TotalPurchases'` column by summing the amount spent on each product by the customer.
  <img width="608" alt="image" src="https://github.com/user-attachments/assets/f55cf3bb-ea82-4e81-87d2-67bc0c2b2e60">

- Created a `'TotalOrders'` column by summing the number of purchases made through all different channels.
  <img width="608" alt="image" src="https://github.com/user-attachments/assets/7c7308f3-0c9b-4e91-a67b-a4c10662c4bc">

  
## :bulb: Demographic Insights:
- **Generation**: The majority of customers are from the `'Gen X'` generation (born between 1965 – 1980).
 <img width="417" alt="image" src="https://github.com/user-attachments/assets/af727e53-bb3d-4797-9e7f-c31885359062">

- **Education**: The majority of customers have a bachelor's degree (column named `'Graduation'`).
- Note: For later visualization purposes, the name `'Graduation'` will be modified to `'Bachelor'`, assuming that is the case.
  
<img width="416" alt="image" src="https://github.com/user-attachments/assets/df9ab6ca-8ce1-4e37-abde-daf7faf79a52">

- **Marital Status**: Most customers are either `'Married'` or `'Together'`.
 <img width="413" alt="image" src="https://github.com/user-attachments/assets/ae1b08a3-d25c-4921-ad22-c7fb0ef8e6ce">


- **Income**: The largest income range for customers is between `$40,000` and `$60,000`, with a count of 652 customers. The number of customers in the `$20,000` - `$40,000` range (592 customers) and the `$60,000` - `$80,000` range (608 customers) are also relatively similar.
 <img width="422" alt="image" src="https://github.com/user-attachments/assets/0a75ef06-b4a7-4228-ac56-8d6df556d0a8">

- **Location**: The majority of customers reside in `Spain`.
  
<img width="417" alt="image" src="https://github.com/user-attachments/assets/619308e7-3d74-4665-9eed-f72f2649e080">

- **Kids at home**: 57.6% of customers have `no childeren`, while 42.4% of the customers have `one` or `two` children.

<img width="419" alt="image" src="https://github.com/user-attachments/assets/20245527-8a7e-496a-a333-1c5d926b8199">

## :bulb: Product Sales Performance:
 - The most popular products are `wines`, followed by `meat` products. This pattern is consistent across different types of customers overall.
 <img width="634" alt="image" src="https://github.com/user-attachments/assets/80ef4178-8fa3-4fde-8890-3dfe7d302d25">


## :bulb: Channel Performance:
- The number of `store` purchases is the highest, followed by the number of `web` purchases. `Deals` and `catalogs` are not as effective in attracting customers.
<img width="602" alt="image" src="https://github.com/user-attachments/assets/34d9b987-98d8-4f03-927e-73b4f6c408cf">

## :bulb: Campaign Results:

 <img width="737" alt="image" src="https://github.com/user-attachments/assets/4c288aee-af61-42d1-9427-399d3917329b">

- Note: The conversion rate is calculated as `(number of customers who accepted the campaign / total number of customers) * 100`.
- Whether the conversion rate is considered good enough depends on industry standards and campaign goals. Since we do not have specific context for this dataset, we can generally compare the latest campaign with previous ones. The third, fourth, and fifth campaigns consistently achieved similar results, around 7.3%. The second campaign performed the worst, with a poor conversion rate of approximately 1.3%. The latest campaign was the most successful, with a conversion rate of about 14.91%.
- Given these results, the company should further investigate what specifically made the latest campaign more successful and focus on replicating that type of campaign to determine if it consistently produces a high conversion rate. Additionally, based on previous lessons, the company should avoid repeating the approach used in the second campaign, which was not effective.

## ⚡ Key Findings and Actionable Recommendations:

### :pushpin: Point 1: Who is the typical customer?

<img width="782" alt="image" src="https://github.com/user-attachments/assets/c45649f2-783c-4f20-9648-4eb2db02d8d5">

- The most common profile:
  - `Gen X` generation (born between 1965 and 1980)
  - Education Level: `bachelor's degree`
  - Marital Status: partnered (either `married` or living `together`)
  - having `no children at home`
  - Annual Income: between `$40,000 and $60,000`
  - Location: `Spain`.
- The company should focus on targeted marketing campaign and content that resonate with this demographic. 

### :pushpin: Point 2: Which Products are Popular?

<img width="700" alt="image" src="https://github.com/user-attachments/assets/0f259e7b-686f-411f-8c13-ec8e8ccd4677">

- The graph shows the amount spent by customers on different product categories in the last two years. In the records, `Wines` are the top revenue-generating category, with a substantial $665K in total spending, highlighting its popularity and significant impact on overall revenue. `Meat` follows as the second-highest category with $396K in spending. In contrast, `Gold`, `Fish`, `Sweet`, and `Fruits` have much lower spending. This suggests a lower level of consumer interest in these product categories or potentially less effective marketing strategies for these products. Therefore, the company should focus marketing efforts on promoting high-revenue categories like `Wines` and `Meat`, while trying different strategies to boost the appeal of lower-performing categories such as `Fruits`.


### :pushpin: Point 3: Which Channels Attract More Customers?
<img width="700" alt="image" src="https://github.com/user-attachments/assets/6b56b2a0-e81c-4dd3-aa4e-6b30d7ef54ad">

- The **Store** channel leads with the highest number of purchases, totaling 12,653. This reflects a strong preference for in-person shopping and the effectiveness of the physical store in driving sales. The **Web** channel is the second most popular channel, with 8,954 purchases, demonstrating significant online engagement but still trailing behind the store. **Catalog** and **Deals** are less effective in attracting customers, with 5,810 purchases and 5,084 purchases, respectively. Therefore, since the majority of customers prefer to buy in person, it may suggest the company should invest in maintaining and potentially expanding the strengths of its store channel. Given the high concentration of customers in **Spain**, opening new stores in this region could be beneficial. And they should enhance the deals channel’s effectiveness through targeted promotions based on customer demographics or by re-evaluating its strategy.


### :pushpin: Point 4: Concern About Customer Recency
<img width="700" alt="image" src="https://github.com/user-attachments/assets/a1212cff-347f-4416-a5f6-df31686a8a3c">

- Note: **Recency** is defined as 'the number of days since a customer's last purchase'.
- The distribution shows that customer purchases are spread out across a wide range of days, from 0 to 100. While some customers are purchasing again within a few days, a significant portion (about **70%**) are taking much longer to return (**more than 30 days**). This indicates that customers do not return quickly, suggesting a need to improve customer retention. Based on this information, the company should utilize targeted marketing campaigns to re-engage customers who have not made a purchase recently. Some possible ways to do it include personalized emails providing special offers like discounts on their next purchase or loyalty programs that reward customers for making frequent purchases. Additionally, it is recommended to conduct a survey to gather feedback from customers to understand why they do not purchase frequently, assess their satisfaction with the products and services, and identify areas for improvement.


### :pushpin: Point 5: Concern About Low Website Visit Frequency
<img width="700" alt="image" src="https://github.com/user-attachments/assets/e73d0174-99da-4c67-a285-da93d670c614">

- Majority of customers visit the website fewer than 10 times a month. This indicates that while there are some repeat visitors, most customers do not visit the site frequently. To increase web purchases, it would be beneficial to encourage more frequent visits. Therefore, the company may need to improve the website’s interface and usability, making it more attractive and easier to navigate. Before making these changes, it would be useful to design a brief and user-friendly questionnaire to understand why customers do not visit the website often and identify specific areas for improvement. This can provide valuable insights for making targeted enhancements to the site.


> [!NOTE]
> For a detailed exploration of the data and results, check out the story in **Tableau** that I delivered visualization of the key findings from this data analysis.\
> To view and interact with the Tableau story, visit [here](https://public.tableau.com/app/profile/sujung.choi/viz/marketing_campaign_17218514702580/Story1)

