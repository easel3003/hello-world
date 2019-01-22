# hello-world
my tutorial repository
# Write your code below and press Shift+Enter to execute
import pandas as pd
import matplotlib.pylab as plt
import numpy as np
filename = "https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DA0101EN/auto.csv"
headers = ["symboling","normalized-losses","make","fuel-type","aspiration", "num-of-doors","body-style",
         "drive-wheels","engine-location","wheel-base", "length","width","height","curb-weight","engine-type",
         "num-of-cylinders", "engine-size","fuel-system","bore","stroke","compression-ratio","horsepower",
         "peak-rpm","city-mpg","highway-mpg","price"]
df = pd.read_csv(filename, names = headers)
df.head()
missing_data = df.isnull()
#missing_data.head(5)
#avg_norm_loss = df["normalized-losses"].astype("float").mean(axis=0)
#print("Average of normalized-losses:", avg_norm_loss)
#avg_stroke = df["stroke"].mean(axis=0)
df.replace("?", np.nan, inplace = True)
avg_stroke = df["stroke"].astype("float").mean(axis=0)
print("Average of Stroke:", avg_stroke)
df["stroke"].replace(np.nan, avg_stroke, inplace=True)
avg_horsepower = df['horsepower'].astype('float').mean(axis=0)
print("Average horsepower:", avg_horsepower)
df['horsepower'].replace(np.nan, avg_horsepower, inplace=True)
avg_peakrpm=df['peak-rpm'].astype('float').mean(axis=0)
print("Average peak rpm:", avg_peakrpm)
df['num-of-doors'].value_counts().idxmax()
df[["bore", "stroke","price", "peak-rpm"]] = df[["bore","peak-rpm","stroke","price"]].astype("float")
# Convert mpg to L/100km by mathematical operation (235 divided by mpg)
df['city-L/100km'] = 235/df["city-mpg"]
df['highway-mpg'] = 235/df["highway-mpg"]
df.rename(columns = {'highway-mpg':'highway-L/100km'}, inplace = True)
#df.rename(columns={'"highway-mpg"':'highway-L/100km'}, inplace=True)
df['height'] = df['height']/df['height'].max()
%matplotlib inline
import matplotlib as plt
from matplotlib import pyplot
df["horsepower"]=df["horsepower"].astype(int, copy=True)
#plt.pyplot.hist(df["horsepower"])
# set x/y labels and plot title
plt.pyplot.xlabel("horsepower")
plt.pyplot.ylabel("count")
plt.pyplot.title("horsepower bins")
#3 bins of equal size bandwidth so we use numpy's linspace(start_value, end_value, numbers_generated function
bins = np.linspace(min(df["horsepower"]), max(df["horsepower"]), 4)
bins
group_names = ['Low', 'Medium', 'High']
#We apply the function "cut" the determine what each value of "df['horsepower']" belongs to
df['horsepower-binned'] = pd.cut(df['horsepower'], bins, labels=group_names, include_lowest=True )
df[['horsepower','horsepower-binned']].head(20)
df["horsepower-binned"].value_counts()
#pyplot.bar(group_names, df["horsepower-binned"].value_counts())
a = (0,1,2)
# draw historgram of attribute "horsepower" with bins = 3
plt.pyplot.hist(df["horsepower"], bins = 3)
dummy_variable_1 = pd.get_dummies(df["fuel-type"])
dummy_variable_1.head()
#dummy_variable_1.rename(columns={'fuel-type-diesel':'gas', 'fuel-type-diesel':'diesel'}, inplace=True)
#dummy_variable_1.head()
df = pd.concat([df, dummy_variable_1], axis=1)
#We would like 3 bins of equal size bandwidth so we use numpy's linspace(start_value, end_value, numbers_generated function
#plt.pyplot.hist(df["horsepower-binned"])
#print(df.dtypes)

# drop original column "fuel-type" from "df"
df.drop("fuel-type", axis = 1, inplace=True)
dummy_variable_2 = pd.get_dummies(df["aspiration"])
dummy_variable_2.rename(columns={'turbo':'std', 'std':'turbo'}, inplace=True)
dummy_variable_2.head()
df = pd.concat([df, dummy_variable_2], axis=1)
df.drop("aspiration", axis = 1, inplace=True)
#df.head(2)
df.to_csv('clean_df.csv')
# df.save('clean_df.csv') - wrong
