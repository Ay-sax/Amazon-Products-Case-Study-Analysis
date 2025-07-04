# Topic: Amazon-Products-Case-Study-Analysis
## Project Overview
You are working as a Junior Data Analyst at RetailTech Insights, a company that provides e-commerce analytics solutions to sellers on platforms like Amazon. Your team has been tasked with analysing product and customer review data to generate insights that can guide product improvement, marketing strategies, and customer engagement. You will use your analysis to answer the following questions:
1. What is the average discount percentage by product category? 
2. How many products are listed under each category? 
3. What is the total number of reviews per category?  
4. Which products have the highest average ratings? 
5. What is the average actual price vs the discounted price by category? 
6. Which products have the highest number of reviews? 
7. How many products have a discount of 50% or more? 
8. What is the distribution of product ratings (e.g., how many products are rated 3.0, 4.0, etc.)? 
9. What is the total potential revenue (actual_price × rating_count) by category? 
10. What is the number of unique products per price range bucket (e.g., <₹200, ₹200–₹500, >₹500)? 
11. How does the rating relate to the level of discount? 
12. How many products have fewer than 1,000 reviews? 
13. Which categories have products with the highest discounts? 
14. Identify the top 5 products in terms of rating and number of reviews combined.

## Data Source
The primary source of data used here is Data_sale.csv and this is an open source data that can be freely downloaded from an open source online such as kaggle or **FRED** or any other data repository file.

## Data Cleaning
Firstly, I imported my dataset into Microsoft Excel as this is the tool we will be utilizing for this analysis.
 I created a duplicate of the dataset so I could still have the original dataset in case I need it. 
Then I proceeded to clean my dataset by placing each column in the appropriate data formats in Excel.
I also capitalized the first letter of each word in my header column and made it bold for uniformity and easy identification.
I removed the columns that were not relevant to my analysis from my dataset which includes: about product, user id, user name, review id, review title, review content, img link and product link.
Each product is meant to have a distinct product id so I checked for duplicate Product Ids using the conditional formatting tool and I discovered that there were duplicated rows in my dataset. This will greatly skew my analysis so I created another sheet in my excel file, Amazon_case_study_v2 where I removed duplicate rows using the remove duplicate tool.
I used the LEFT and FIND function to extract the category names from the Category column.
=LEFT(C2, FIND("|",C2))
 Then I used the find and replace tool to rename these categories thereby replacing the ambiguous   names in the old Category column. 
I restructured the product names due to inconsistencies in the use of capital letters.
=PROPER(B2)
I made sure all Product Ids had 10 characters in them.
=LEN(A2)
This gave me the number of characters in each cell under Product_Id on a different column and then I used conditional formatting to make sure all cell values were equal to 10.
I changed the Discount (%) column to numbers first by changing the data type from percentage to numbers and then multiplying by 100.
I used the filter tool to search for missing values. I found two in the Rating_Count column which I deleted for the sake of accuracy during analysis.
I also noticed an inconsistency in my Ratings column where a product did not have a rating but a symbol so I deleted the product’s data so it does not skew my analysis because I could not contact the company to trace the error.
I also converted my dataset to a table for easy analysis.

Exploratory Data Analysis
Firstly, I created a pivot table on another sheet named Pivot Tables. Then to get the average discount percentage for each product category, I put the Product Category column on the rows section of the pivot table and Discount (%) in the values section. Then I set the metrics I was calculating to Average. I also put a title for the table and renamed the column headers.
Then, I created another pivot table in the same sheet to get the Number of Products in each product category. I put the Product Category column on the rows section of the pivot table and Product Id in the values section. Then I set the calculation metrics to Count.
To get the Number of Reviews per Product Category, I created another pivot table and put Category in the row section and Rating Count in the values section with sum as the calculation metric.
To get the Highest Rated Products, I put Category in the row section of my pivot table and the Rating column in the values section. Then I set my calculation metrics to average, then I sorted my pivot table in descending order (largest to smallest) based on the Average Rating. Then I went further to filter my table to give me the top 10 highest rated products.
Then I also proceeded to get the average discount and actual price for each product category using pivot table. I added product category to the rows section, then I added Discount and Actual price columns to the values section and set average as the calculation metrics for both columns.
To get the products with the highest number of reviews, I added the Product Name column to the row section of my pivot table and Rating Count column to the values section with sum as my calculation metrics. Then I sorted my pivot table in descending order (largest to smallest) by rating count and then I filtered it to show the top 10 products with the highest number of reviews.
To get the number of products with at least 50% discount, I created a column Discount>=50 in my worksheet using the IF function.
=IF(F2>=50,"1","0")
Then I created a pivot table and put my created column ‘Discount>=50’ in the values section. Then I set my calculation metrics to sum.
NOTE: Pivot tables are likely to regard cells with formulas as text, so I usually copy my results and paste them as ordinary numbers so they could be easily recognized for calculations.
To get the Distribution of Product Ratings using my pivot table, I created a column Rating (Rounded) so I could put all ratings in classes (1.0, 2.0, 3.0, etc). I am using an older version of Excel so I had to achieve this using the IF function instead of the IFS function which is ideal for writing a code with multiple conditions. This is how I wrote my IF function:
=IF(G2= "NULL", "NULL", IF(G2<=1.4,"1.0",IF(G2<=2.4, "2.0", IF(G2<=3.4, "3.0", IF(G2<=4.4, "4.0", IF(G2>=4.5, "5.0"))))))
Then I created a new pivot table and added the Rating (Rounded) column to the row section of my pivot table and Product Id to the value section with count as my calculation metric.
Next, I created a pivot table to show the potential revenue for each product category. Firstly, I created a calculated column Potential Revenue to get the potential revenue for each product:
=E2*H2
Then I added Product Category column to the row section of my pivot table and Potential Revenue to the value section with sum as my calculation metric to get the total potential revenue each product category could generate.
I created a column, Price Range for further analysis. I created this column using the IF function as follows:
=IF(D2<200,"<₹200", IF(D2<=500, "₹200–₹500", " >₹500"))
To analyze the number of products in each price range, I created a pivot table and added the Price Range column to the row section of the pivot table. Then I added the Product Id column to the values section of the pivot table and used count as my calculation metric so that I could know the number of products in each price range bucket.
To use pivot table to get the number of products with less than 1000 reviews, I created a column, <1000 Reviews, then I used the IF function:
=IF(H2<1000, "1", "0")
This created a column with 1 for every product that had less than 1000 reviews and 0 for otherwise.
Then I created a pivot table, put the <1000 Reviews column in the values section and did a sum to give me the number of products with less than a 1000 reviews.
To analyze the top 5 products in terms of ratings and number of reviews combined, I created a new column called Score(Rating * Rating_Count)/1000:
=(G2 * H2)/1000
Then I created a pivot table adding Product Name column to the row section and Rating, Rating Count and Score column to the values section. Then I sorted the pivot table by Score (descending order) and filtered the pivot table to get the top 5 products.

I also analyzed my data to check if Discount levels affects ratings. So firstly, I created a new column called Discount Group:
=IF(F2<=20,"0-20%", IF(F2<=40, "20-40%", IF(F2<=60, "40-60%", IF(F2<=80, "60-80%",IF(F2<=100, "80- 100%")))))
Then I created a pivot table using Discount Group in the row section and Rating in the value section with my calculation metric set to Average.



Visualization
*Insert picture (dashboard)
Findings
-	We had 1,348 products with an average rating of 4.1 and an average discount of 46.7%.
-	I also discovered that products under the Home Improvement category had the highest discounts and the Electronics category had the most products. 
-	Amazon wireless mouse and Syncwire ltg to usb cable appears to be the top rated products across all categories with a rating of 5.0.
-	About 1,213 out of the 1,348 products on Amazon are rated 4.0 or above which attests to good quality of items sold in the store.
-	Also, more than half of the products been sold at Amazon cost more $500 which could mean that the products are quite expensive (depending on what is being sold).

*Insert supporting visual
- Electronics & Accessories had the most reviews among all product categories. This signal popularity of products in that category or trust in the product’s efficacy.
-  Some Amazon cables had the highest reviews in relation to any other products. This could signify satisfaction and trust in this product. It could also mean that customers really love the product. Also, most of the products with the highest numbers of reviews are in the Electronics & Accessories category.
-  Also, the Electronics & Accessories product category has the potential of generating the most revenue. This could be due to people’s trust and satisfaction with the products in that category as stated earlier.
*Insert pic of number of products with >= 50% discount.
About 660 products has over 49% discount placed on them.

*Insert number of products with < 1000 Reviews
307 products had less than 1000 reviews. This could mean that a lot of people do not buy those products or are dissatisfied with them.
*Insert top  products in terms of review and ratings combined.
From this, we could say these are the top five most loved and enjoyed products reading from the input of customers.





*Insert supporting visuals 2
From my results, it suggests that higher discount levels are associated with slightly lower average ratings. While the decline is very minimal, it may indicate that heavily discounted products receive slightly less favorable ratings, possibly due to lower quality or customer expectations.



