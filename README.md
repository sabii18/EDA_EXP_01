# Experiment 1: EDA in IPL Dataset

## Aim:

To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and derive insights about matches per season, winning teams, toss decisions, and top venues.

## Algorithm:

### 1.Import Libraries
Import pandas for data handling. Import matplotlib and seaborn for visualization.

### 2.Load Dataset
Use pd.read_csv() to load the IPL matches dataset. Check dataset shape using .shape. View first 5 rows using .head().

### 3.Matches per Season (Univariate Analysis)
Group data by season and count matches. Plot a bar chart to visualize growth/decline in matches.

### 4.Top Winning Teams (Univariate Analysis)
Use value_counts() on the winner column. Plot top 5 winning teams in a bar chart.

### 5.Toss Decisions (Univariate Analysis)
Count toss decision preferences (bat vs field). Plot results using a bar chart.

### 6.Top Venues (Univariate Analysis)
Count matches per venue. Display top 5 venues with a horizontal bar chart.

### 7.Draw Insights
Observe patterns in toss decisions. Identify teams with consistent winning trends.

## Program:

```python

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("matches.csv")

df.shape
df.head()

df.info()
df.dtypes
df.columns

df["id"].is_unique

df.isnull().sum()

df.duplicated().sum()

df["city"] = df["city"].fillna(df["city"].mode()[0])

df.drop_duplicates(inplace=True)

season_matches = df.groupby("season").size()
print(season_matches)

print("Season with highest matches:", season_matches.idxmax())
print("Highest number of matches:", season_matches.max())

season_matches.plot(kind="bar")
plt.title("Matches per Season")
plt.xlabel("Season")
plt.ylabel("Number of Matches")
plt.show()

team_wins = df.groupby("winner").size().sort_values(ascending=False)
print(team_wins)

top5 = team_wins.head(5)
print(top5)

top5.plot(kind="bar")
plt.title("Top 5 Teams by Wins")
plt.xlabel("Teams")
plt.ylabel("Wins")
plt.show()

csk = df[df["winner"] == "Chennai Super Kings"]
print(csk)
print("Total CSK Wins:", len(csk))

toss = df["toss_decision"].value_counts()
print(toss)

percentage = df["toss_decision"].value_counts(normalize=True) * 100
print(percentage)

print("Most Preferred Toss Decision:", percentage.idxmax())
print("Percentage:", percentage.max())

toss.plot(kind="bar")
plt.title("Toss Decision Preference")
plt.xlabel("Decision")
plt.ylabel("Count")
plt.show()

cross = pd.crosstab(df["season"], df["toss_decision"])
print(cross)

top5_venues = df["venue"].value_counts().head(5)
print(top5_venues)

top5_venues.plot(kind="bar")
plt.title("Top 5 Venues")
plt.xlabel("Venue")
plt.ylabel("Matches")
plt.show()

print("Largest Winning Margin:")
print(df[df["win_by_runs"] == df["win_by_runs"].max()])

top10 = df.sort_values(by="win_by_runs", ascending=False).head(10)
print(top10)

result = df["result"].value_counts()
print(result)

df["date"] = pd.to_datetime(df["date"])

df["year"] = df["date"].dt.year

def win_type(row):
    if row["result"] == "tie":
        return "Tie"
    elif row["result"] == "no result":
        return "No Result"
    elif row["win_by_runs"] > 0:
        return "Won by Runs"
    elif row["win_by_wickets"] > 0:
        return "Won by Wickets"
    else:
        return "Unknown"

df["win_type"] = df.apply(win_type, axis=1)

print(df[["date", "year", "result", "win_by_runs", "win_by_wickets", "win_type"]].head())

```

## Output:

<img width="1493" height="622" alt="Screenshot 2026-07-23 221856" src="https://github.com/user-attachments/assets/db4c6a3e-3f12-460e-8d80-9044d0aedfb9" />

<img width="1493" height="661" alt="Screenshot 2026-07-23 221911" src="https://github.com/user-attachments/assets/90de37df-2bfe-43b8-b1e7-a4395b4be91b" />

<img width="1490" height="715" alt="Screenshot 2026-07-23 221943" src="https://github.com/user-attachments/assets/056f1bdf-208e-4edf-81dc-b0f30018f04f" />

<img width="1492" height="745" alt="Screenshot 2026-07-23 221953" src="https://github.com/user-attachments/assets/0b53f193-0964-4e23-bc0e-ff3fef93c723" />

<img width="1487" height="657" alt="Screenshot 2026-07-23 222012" src="https://github.com/user-attachments/assets/9be07636-24d5-49ce-b5db-348a53ecfa41" />

<img width="1490" height="778" alt="Screenshot 2026-07-23 222021" src="https://github.com/user-attachments/assets/e2b6285b-b80f-4387-adaf-ced29de41d8b" />

<img width="1487" height="575" alt="Screenshot 2026-07-23 222030" src="https://github.com/user-attachments/assets/920f6cbd-213d-46b7-b5c7-403b8be03ddd" />

<img width="1486" height="232" alt="Screenshot 2026-07-23 222042" src="https://github.com/user-attachments/assets/81c6b743-a2c5-467a-9db5-c4e6642af388" />

<img width="1342" height="790" alt="Screenshot 2026-07-23 222110" src="https://github.com/user-attachments/assets/534f5573-b9f9-47cc-9be5-5ee8387a7afd" />

<img width="1338" height="765" alt="Screenshot 2026-07-23 222135" src="https://github.com/user-attachments/assets/11d4420a-932f-4be3-b1ca-c07db837cecc" />

<img width="1332" height="588" alt="Screenshot 2026-07-23 222149" src="https://github.com/user-attachments/assets/ee82dcb0-6ce3-4cc4-9883-9dce66986b29" />

<img width="1337" height="722" alt="Screenshot 2026-07-23 222158" src="https://github.com/user-attachments/assets/4418cb56-77ea-4551-ad2a-de70b363c080" />

<img width="1335" height="766" alt="Screenshot 2026-07-23 222208" src="https://github.com/user-attachments/assets/f721dd8d-3c2d-4182-8cd3-a40a717dcb3b" />

<img width="1337" height="572" alt="Screenshot 2026-07-23 222231" src="https://github.com/user-attachments/assets/1cfe3d4c-c805-4df4-a8ec-de6b87ebcbc1" />

<img width="1336" height="722" alt="Screenshot 2026-07-23 222241" src="https://github.com/user-attachments/assets/23c307c3-f80c-4ab3-b9aa-57ccfa22c0ef" />

## Result:

The IPL matches.csv dataset was successfully analyzed using Pandas by performing data cleaning, grouping, filtering, sorting, visualization, and transformation operations. The required outputs were obtained successfully for all the given tasks.
