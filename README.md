# ==============================
# SMART ELECTRICITY ANALYSIS PROJECT
# ==============================

# Import Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# ------------------------------
# Step 1: Create Dataset
# ------------------------------
np.random.seed(42)

data = {
    "Date": pd.date_range(start="2025-01-01", periods=180),
    "Units_Consumed": np.random.randint(5, 25, 180),
    "Temperature": np.random.randint(20, 40, 180)
}

df = pd.DataFrame(data)

# Cost calculation (₹6 per unit)
df["Cost"] = df["Units_Consumed"] * 6

# ------------------------------
# Step 2: Feature Engineering
# ------------------------------
df["Month"] = df["Date"].dt.month

def get_season(month):
    if month in [3,4,5,6]:
        return "Summer"
    elif month in [7,8,9]:
        return "Monsoon"
    else:
        return "Winter"

df["Season"] = df["Month"].apply(get_season)

# ------------------------------
# Step 3: Basic Analysis
# ------------------------------
print("Total Units Consumed:", df["Units_Consumed"].sum())
print("Total Electricity Cost (₹):", df["Cost"].sum())
print("Average Daily Consumption:", round(df["Units_Consumed"].mean(), 2))

# ------------------------------
# Step 4: Monthly Analysis
# ------------------------------
monthly_usage = df.groupby("Month")["Units_Consumed"].sum()

plt.figure()
monthly_usage.plot(kind="bar")
plt.title("Monthly Electricity Consumption")
plt.xlabel("Month")
plt.ylabel("Total Units")
plt.show()

# ------------------------------
# Step 5: Seasonal Analysis
# ------------------------------
seasonal_usage = df.groupby("Season")["Units_Consumed"].sum()
print("\nSeasonal Usage:")
print(seasonal_usage)

# ------------------------------
# Step 6: Correlation Analysis
# ------------------------------
correlation = df["Units_Consumed"].corr(df["Temperature"])
print("\nCorrelation between Temperature and Units:", round(correlation, 2))

# ------------------------------
# Step 7: Save File for Tableau
# ------------------------------
df.to_csv("Electricity_Consumption_Data.csv", index=False)

print("\nProject Completed Successfully 🚀")
