# PA4_MUSONG_2ECEA

#### Purpose
Pandas specifically Data Wrangling and Data Visualization to clean and filter the ECE Board Exam dataset and matplotlib to visualize how Track, Gender, and Hometown relate to the computed Average.

#### Part 1 - Create the required DataFrames
### Dataset needed
```` board2.xlsx ```` (place it in the same folder as your script/notebook).

````
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_excel("board2.xlsx")
score_cols = ["Math", "GEAS", "Electronics", "Communication"]
df[score_cols] = df[score_cols].apply(pd.to_numeric, errors="coerce")
df["Average"] = df[score_cols].mean(axis=1)

````
I loaded the dataset board2.xlsx into a DataFrame and made sure the subject scores were numeric using ````.apply(pd.to_numeric)````. I then created a new column Average using ````.mean(axis=1)````, which computes the row-wise average. This is much easier than writing loops.

### What each line does

```` import pandas as pd: ```` loads pandas for data wrangling.

```` import matplotlib.pyplot as plt: ```` loads matplotlib for charts.

```` pd.read_excel("board2.xlsx"): ```` reads the Excel file into a DataFrame called df.

```` score_cols = [...]: ```` lists the subject columns we’ll average.

```` apply(pd.to_numeric, errors="coerce"): ```` converts those columns to numbers; invalid values become NaN so math won’t crash.

````mean(axis=1):```` computes row-wise average across the 4 subjects and stores it as a new column Average.


### Instru DataFrame

````
Instru = df.query(
    "Track == 'Instrumentation' and Hometown == 'Luzon' and Electronics > 70"
)[["Name", "GEAS", "Electronics"]]
Instru.to_csv("Instru.csv", index=False)

````
I created two special DataFrames. The Instru DataFrame filtered Instrumentation students from Luzon with Electronics > 70, and saved only their Name, GEAS, and Electronics. I used ````.query()```` because it makes conditions more readable than combining multiple Boolean checks.

### What each line does

```` df.query("..."):```` picks only rows that match all three conditions.

````[["Name","GEAS","Electronics"]]: ```` keeps just those three columns in the result.

```` to_csv(..., index=False): ```` saves a clean CSV without the pandas row index.

### Mindy DataFrame

````
Mindy = df.query(
    "Hometown == 'Mindanao' and Gender == 'Female' and Average >= 55"
)[["Name", "Track", "Electronics", "Average"]]
Mindy.to_csv("Mindy.csv", index=False)
````
The Mindy DataFrame filtered female students from Mindanao with an Average ≥ 55, keeping their Name, Track, Electronics, and Average. I used ````.query()```` because it makes conditions more readable than combining multiple Boolean checks.

### What each line does

```` df.query("..."):```` filters Mindanao + Female + passing Average.

Column selection matches the required output.

Exports to ````Mindy.csv ```` for submission.

#### PART 2 - Visualizations (contribution to Average)

### Average by Track, Gender and Hometown
````
df.groupby("Track")["Average"].mean().plot(kind="bar", title="Average by Track")
plt.ylabel("Average"); plt.show()

df.groupby("Gender")["Average"].mean().plot(kind="bar", title="Average by Gender")
plt.ylabel("Average"); plt.show()

df.groupby("Hometown")["Average"].mean().plot(kind="bar", title="Average by Hometown")
plt.ylabel("Average"); plt.show()
````
For visualization, I grouped the data by Track, Gender, and Hometown using ````.groupby()```` and plotted bar charts of their average scores. This gave a clear view of differences between groups without calculating manually.

### What each line does

```` groupby("Track"): ```` groups rows by Track.

```` ["Average"].mean():```` average of Average per Track.

````plot(kind="bar"):```` bar chart of those means.

````plt.show():```` displays the figure.

#### MAIN POINTS
- Track differences: Point to the “Average by Track” chart and mention which tracks are highest/lowest.

- Gender/Hometown: Note whether differences look large or probably sample-size noise.

- Interpretation: Suggest plausible reasons (curriculum focus, access to review centers, etc.), but keep it cautious unless you have more evidence.
