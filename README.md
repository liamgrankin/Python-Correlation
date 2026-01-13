###


First I took a look at the data and noticed some null values. 
<img width="266" height="282" alt="image" src="https://github.com/user-attachments/assets/34607f32-8caf-45ee-9616-87e65e1d67fb" />

To clean the table I removed the null values from items that will likely effect later analysis and updated the table. I also dropped any duplicates from the table:
````
for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}'.format(col,pct_missing))
<img width="568" height="449" alt="image" src="https://github.com/user-attachments/assets/49fd53b0-2e58-45f4-b56b-6c13d287aa75" />
````
````
df = df.dropna(subset=["budget", "gross"])
````
To standardize the data I made the <code>budget</code> and <code>gross</code> columns of <code>int64</code> type.
````
df["budget"] = df["budget"].astype('int64')
df["gross"] = df["gross"].astype('int64')
````
When looking at the data I noticed that the year listed in the <code>released</code> column didn't always match the <code>year</code> column:

<img width="432" height="633" alt="Screenshot 2026-01-13 163632" src="https://github.com/user-attachments/assets/fb3abdea-eeb0-4c20-831e-e01a9b4fa521" />

To fix this, I first extracted the year from the <code>released</code> column and compared it with the <code>year</code> values then corrected the 
<code>year</code>

````
df["clean_date"] = df["released"].str.replace(r"\s*\(.*?\)", "", regex=True)
df["clean_date"] = df["clean_date"].str[-4:]
df["clean_date"] = pd.to_numeric(df["clean_date"], errors="coerce")
df["year_match"] = df["clean_date"] == df["year"]
df[df["year_match"] == False]
df["year"] = df["clean_date"]

````
### Finding correlations

To find any correlations I first sorted the data and decided what columns were likely to have strong correlations.
````
df.sort_values(by=['gross'], inplace = False, ascending = False)
````
Based on the available information I hypothesized that the <code>budget</code> and <code>gross</code> columns should be strongly correlated, so I 
attempted to view the data in the form of a scatterplot:
````
plt.scatter(x=df['budget'],y=df['gross'])
plt.title('Budget vs Gross Earnings')
plt.xlabel('Gross Earnings')
plt.ylabel('Budget for Film')
plt.show()
````

<img width="568" height="457" alt="image" src="https://github.com/user-attachments/assets/f7cb663d-c2f6-418d-acfa-683e3b95361d" />

To better examine the information I then added a regression analysis using the <code>seaborn</code> module:

````
sns.regplot(x='budget',y='gross',data=df, scatter_kws={"color":"red","s":4},line_kws={"color":"blue"})
````

<img width="568" height="449" alt="image" src="https://github.com/user-attachments/assets/88d3987d-5f2a-46bf-bb5a-724a266b8ae3" />

