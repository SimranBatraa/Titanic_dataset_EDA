# Titanic_dataset_EDA
--Getting an overview of the data set

df=pd.read_csv('/content/drive/MyDrive/Mobile price New - Mobile price New.csv')

---Finding the size of the dataset

df.shape

---Finding the datatypes of the various columns

df.info()

There are 4 columns which are wrongly classified as object,though they are string datatypes

---Finding the statistical summary of all columns

df.describe()

---Cleaning the columns where datatypes are not correct.Since there were noise values in these columns,therefore they were not being able to convert to int datatypes.
Therefore,used errors='coerce' to ease the conversion.For a permanent effect,we can assign the column to the query,or use inplace= True also.

df['battery_power']=pd.to_numeric(df['battery_power'],errors='coerce')
df['mobile_wt']=pd.to_numeric(df['mobile_wt'],errors='coerce')
df['px_width']=pd.to_numeric(df['px_width'],errors='coerce')
df['ram']=pd.to_numeric(df['ram'],errors='coerce')

--- Finding the number of null values

df.isnull().sum()

Found out that around 1600 values were null out of 2000 rows,dropping is not a feasible option,hence replaced these values by mean of that column

df['ram'].fillna(df['ram'].mean(),inplace=True)

---Dropping other null values of different columns as there are only maximum 4 null values

df.dropna(inplace=True)

---Outlier analysis

Checking outliers via boxplots

num_col=['battery_power', 'blue', 'clock_speed', 'dual_sim', 'fc', 'four_g',
       'int_memory', 'm_dep', 'mobile_wt', 'n_cores', 'pc', 'px_height',
       'px_width', 'ram', 'sc_h', 'sc_w', 'talk_time', 'three_g',
       'touch_screen', 'wifi', 'price_range']
for c in num_col:
  plt.figure()
  sns.boxplot(y=c,data=df)
  plt.show()
  
---Removing outliers

h=['fc','px_height','three_g','ram']
for i in h:
  percentile25=df[i].quantile(0.25)
  percentile75=df[i].quantile(0.75)
  iqr=percentile75-percentile25
  upper_limit=percentile75+(1.5*iqr)
  lower_limit=percentile25-(1.5*iqr)
  df=df[(df[i]<upper_limit) & (df[i]>lower_limit)]
  plt.figure()
  sns.boxplot(y=i, data=df)
