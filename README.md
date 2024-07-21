# :computer: Marketing Campaign Data Analysis
In this project, I used Microsoft Excel to explore and analyze Maven marketing campaign data, containing 2,240 customers' information.

🌼 Dataset Link: [Maven Analytics](https://www.mavenanalytics.io/data-playground?order=date_added%2Cdesc&search=marketing)

The main objectives were to:
- Identify customer segmentation based on their demographics (e.g., age, education, marital status) and the types of products they buy the most (e.g., meat, fish, wines) to better understand customers and target main customers with customized services.
- Evaluate the effectiveness of past marketing campaigns (AcceptedCmp1-5 and Response in the given dataset).
- Evaluate the performance of different sales channels (e.g., web, catalog, store) and identify areas for improvement in the company's online and offline sales strategies.

## 📚 Dataset Description:

![image](https://github.com/user-attachments/assets/51f94ff5-83ce-4cd6-b7c8-3cb433af2a97)


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
- Ordered the `'Year_Birth'` column chronologically.
- Created a new `'Age'` column using the formula: `=YEAR(TODAY()) - D2`
  ![image](https://github.com/user-attachments/assets/32e4f5e8-504d-48f3-bff8-f78d2bf6b18c)

- Added a `'Generation'` column with the formula:
  ```
  =IF(C2>=12,IF(C2<=27,"Gen Z",IF(C2<=43,"Millennials",IF(C2<=59,"Gen X",IF(C2<=69,"Boomers II",IF(C2<=78,"Boomers I",IF(C2<=96,"Post War",IF(C2<=102,"WWII","Unknown"))))))),"Unknown")

  ```

  ![image](https://github.com/user-attachments/assets/911041fb-e0fb-482f-8a0d-16151611b43a)

- Created income bins with $10,000 intervals in the `'Income'` column, which resulted in 11 bins.
  ![image](https://github.com/user-attachments/assets/b414630c-a0a1-4443-8f24-2ac29fc581bd)
  ![image](https://github.com/user-attachments/assets/10537ef3-2b79-401d-80ce-57b751864396)

 
- However, later, the bins were adjusted to $20,000 intervals to reduce the number of bins.

  ![image](https://github.com/user-attachments/assets/eed3e58d-d85b-458c-bc86-21ac096420b8)

- Moreover, the `'Above 100000'` bin was combined with the `'80001 – 100000'` bin to form the `'Above 80000'` bin due to the small number of people in the former.

## :bulb: Demographic Insights:
- **Generation**: The majority of customers are from the `'Gen X'` generation (born between 1965 – 1980).
 <img width="417" alt="image" src="https://github.com/user-attachments/assets/af727e53-bb3d-4797-9e7f-c31885359062">

- **Education**: The majority of customers have a bachelor's degree (column named `'Graduation'`).
  
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
- The number of `store` purchases is the highest, followed by the number of `web` purchases. `Deals` and `catalogs` are underperforming in attracting customers.
<img width="602" alt="image" src="https://github.com/user-attachments/assets/34d9b987-98d8-4f03-927e-73b4f6c408cf">

## :bulb: Campaign Results:
- Whether the conversion rate is considered good enough depends on industry standards and campaign goals. Since we do not have specific context for this dataset, we can generally compare the latest campaign with previous ones. The third, fourth, and fifth campaigns consistently achieved similar results, around 7.3%. The second campaign performed the worst, with a poor conversion rate of approximately 1.3%. The latest campaign was the most successful, with a conversion rate of about 15%.
- Given these results, the company should further investigate what specifically made the latest campaign more successful and focus on replicating that type of campaign to determine if it consistently produces a high conversion rate. Additionally, based on previous lessons, the company should avoid repeating the approach used in the second campaign, which was not effective.

 <img width="737" alt="image" src="https://github.com/user-attachments/assets/4c288aee-af61-42d1-9427-399d3917329b">

## Conclusion:

   
> [!NOTE]
> For a detailed exploration of the data and results, check out the story in **Tableau** that I delivered visualization of the key findings from this data analysis.\
> To view and interact with the Tableau story, visit [here]()

