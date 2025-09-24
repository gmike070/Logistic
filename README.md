# e-commerce company with integrated logistics operations

## Objective

To analyze customer orders, loyalty, and operational efficiency in order to identify sales patterns, customer retention drivers, and operational bottlenecks, ultimately helping the business improve revenue, reduce churn, and enhance customer satisfaction.

## Problem Statements


1. Which product categories contribute the most to sales, and how can the company focus marketing strategies accordingly?

2. Does applying a coupon code significantly affect customer loyalty scores, and what does this mean for promotional strategies?

3. Which regions or cities show the highest return rates, and how can logistics or product quality be improved to minimize returns?

## Setting up the environment
Installing the required libraries:

•	We need Numpy for mathematical operation

•	Pandas for Dataframe Manipulation

•	Seaborn and Matplotlib for data visualization

•	And finally Jupyter Notebook to build an interactive ipython notebook

Python Code (Step-by-Step)
Step 1: Import Libraries
 ```python
    import pandas as pd
    import matplotlib.pyplot as plt
 ```

Step 2: Load and Inspect the Dataset Python:
```
# let read the dataset
data = pd.read_csv("C:\\Users\\Admin\\Desktop\\DATA COURSE\\eda_practice_dataset.csv")

data.head(10)
```
Output:

<img width="626" height="323" alt="image" src="https://github.com/user-attachments/assets/8a196e7f-9968-4f93-929b-34bf8b848257" />

Step 3: Data Cleaning & Preprocessing Python code:
```
# DATA CLEANING

# missing data
# lets import warnings module

import warnings
warnings.filterwarnings('ignore')


# lets calculate the total missing values

total = data.isnull().sum().sort_values(ascending = False)

# lets calculate the percentage of missing values in the data

percent = ((data.isnull().sum()/data.isnull().count())*100).sort_values(ascending = False)


# lets store the above two values in a dataset called missing data

missing_data = pd.concat([total, percent], axis=1, keys=['total', 'percent %'])

# lets check the head of the data
missing_data
```
Output:

<img width="176" height="362" alt="image" src="https://github.com/user-attachments/assets/78b8825f-79f8-4ab5-b81b-e6ceb2499f71" />

```
# Convert Date column to datetime format
data["OrderDate"] = pd.to_datetime(data["OrderDate"], errors="coerce")
data["CustomerSince"] = pd.to_datetime(data["CustomerSince"], errors="coerce")


# Drop rows with null dates
data = data.dropna(subset=['OrderDate'])
data = data.dropna(subset=['CustomerSince'])

print(data.dtypes)
data.head(10)
```
Output:

<img width="218" height="288" alt="image" src="https://github.com/user-attachments/assets/c30cac01-4143-403e-9913-5936080ef863" />

<img width="622" height="312" alt="image" src="https://github.com/user-attachments/assets/d07a9ced-fc4c-495d-b8f8-e1d018c207af" />

```
# lets input the missing values


# using mean for numerical columns
data['Quantity'].fillna(data['Quantity'].mean(),inplace = True)

# using mode for categorical column
data['Price'].fillna(data['Price'].mode()[0], inplace = True)


# As we input all the missing values let check the number of the total missing values in the dataset
data.isnull().sum().sum()

print(data.isnull().sum())        
```
Output: 

<img width="145" height="293" alt="image" src="https://github.com/user-attachments/assets/f422c090-2f65-42fc-bb2f-d45f08565f60" />


## Analysis 1 Top product categories by total sales 

Python code:
```
# Top product categories by total sales 

top_categories_sales = (
    data.groupby("ProductCategory")["TotalSale"].sum()
    .sort_values(ascending=False)
    .head(5)
)


print("Top Product Categories by Sales:\n", top_categories_sales)

plt.figure(figsize=(8,5))
top_categories_sales.plot(kind="bar", color="skyblue", edgecolor="black")
plt.title("Top 5 Product Categories by Total Sales")
plt.ylabel("Total Sales")
plt.xlabel("Product Category")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

Output:

<img width="524" height="359" alt="image" src="https://github.com/user-attachments/assets/d29c935d-353b-42af-9ca3-ec2d1f3c68cb" />

## Analysis 2 Coupon vs No Coupon LoyaltyScore 
Python code:
```
#  Coupon vs No Coupon LoyaltyScore 
loyalty_coupon = (
    data.groupby(data["CouponCode"].notnull())["LoyaltyScore"].mean()
    .rename(index={False: "No Coupon", True: "Coupon Applied"})
)

print("\nLoyalty Score (Coupon vs No Coupon):\n", loyalty_coupon)

plt.figure(figsize=(6,4))
loyalty_coupon.plot(kind="bar", color=["orange", "green"], edgecolor="black")
plt.title("Average Loyalty Score: Coupon vs No Coupon")
plt.ylabel("Average Loyalty Score")
plt.xlabel("Coupon Usage")
plt.xticks(rotation=0)
plt.tight_layout()
plt.show()

```
Output:

<img width="479" height="263" alt="image" src="https://github.com/user-attachments/assets/ff553636-5183-4220-a2b5-13deb218f24a" />

## Analysis 3 Return rates by Region

Python code:
```
# Return rates by Region 
return_rates = (
    data.groupby("Region")["ReturnFlag"].apply(lambda x: (x == "Yes").mean())
    .sort_values(ascending=False)
)

print("\nReturn Rates by Region:\n", return_rates)  

plt.figure(figsize=(8,5))
return_rates.plot(kind="bar", color="salmon", edgecolor="black")
plt.title("Return Rates by Region")
plt.ylabel("Return Rate")
plt.xlabel("Region")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
Output:

<img width="548" height="382" alt="image" src="https://github.com/user-attachments/assets/7d534867-c739-4447-838c-bfdafc165884" />


## Recommendations

I. For Product Sales: Focus marketing campaigns on the top 2–3 performing categories, while also investigating why lower-selling categories underperform (pricing, demand, or distribution issues).

II. For Coupons & Loyalty: Since coupon users show higher loyalty scores, consider targeted coupon campaigns for low-loyalty or high-churn-risk customers.

III. For Returns: In regions/cities with high return rates, investigate product quality issues, delivery problems, or mismatched customer expectations. Partner with logistics teams to optimize shipping methods and reduce delays.

## Conclusion

This analysis highlights how sales, loyalty, and returns vary across products, promotions, and regions. By leveraging coupons strategically, focusing on best selling categories, and addressing return heavy areas, the company can increase revenue, improve customer retention, and reduce operational inefficiencies.
