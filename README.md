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
