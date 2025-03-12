import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
file_path = "sales_data_sample.csv"  # Update with actual path
sales_data = pd.read_csv(file_path, encoding="latin1")

# Convert ORDERDATE to datetime format
sales_data["ORDERDATE"] = pd.to_datetime(sales_data["ORDERDATE"], errors="coerce")

# Fill missing values
sales_data["STATE"].fillna("Unknown", inplace=True)
sales_data["POSTALCODE"].fillna("00000", inplace=True)
sales_data["TERRITORY"].fillna("Not Assigned", inplace=True)

# Display summary statistics
print("Summary Statistics:")
print(sales_data.describe())

# Sales Trend Over Time
plt.figure(figsize=(12, 6))
sales_data.groupby("ORDERDATE")["SALES"].sum().plot(marker="o", linestyle="-")
plt.title("Total Sales Over Time")
plt.xlabel("Order Date")
plt.ylabel("Total Sales ($)")
plt.xticks(rotation=45)
plt.show()

# Sales by Month
plt.figure(figsize=(10, 5))
sns.barplot(x=sales_data["MONTH_ID"], y=sales_data["SALES"], estimator=sum, palette="coolwarm")
plt.title("Total Sales by Month")
plt.xlabel("Month")
plt.ylabel("Total Sales ($)")
plt.show()

# Top 10 Best-Selling Products
top_products = sales_data.groupby("PRODUCTLINE")["SALES"].sum().sort_values(ascending=False)
plt.figure(figsize=(12, 5))
sns.barplot(x=top_products.index, y=top_products.values, palette="viridis")
plt.title("Total Sales by Product Line")
plt.xlabel("Product Line")
plt.ylabel("Total Sales ($)")
plt.xticks(rotation=45)
plt.show()
