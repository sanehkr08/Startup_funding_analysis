
# Startup funding Dataset Analysis

One of my friend developed a product and wants to build a startup company for the product. But he does not have enough capital to establish the company hence, he is seeking for investments. He requested me to analyse the startup finding dataset and bring out useful insight that help him in establishing his company. 


## Tools used
- Programming Language : Python
- Data Analysis Libraries : Pandas and NumPy
- Data Visualization Library : Matplotlib

## About Data
The startup funding dataset is a table of startups that was being funded by angels and firms from 2015 to 2020. The table contain

- Name of startup
- City (where it was founded)
- Investors Name
- Industry Sector
- Type of Invesment
- Amount funded

## Problems tackled
### Find out the best city to establish the company

```python
city_data = np.array(df.City) # converting the city column into numpy array
# cities name has multiple names separateds by "/", using split() to separate required cities
cities = []
for i in city_data:
    if i != np.nan:
        x = str(i).split('/')
        for k in x:
            if k[0] == ' ':
                k = k[1:]
            if k[-1] == ' ':
                k = k[:-1]
            cities.append(k)
            
Cities = np.array(cities)
#counting the number of cities usinf len() for Bangalore , Mumbai and NCR
bn = len(np.where(Cities == "Bangalore")[0])
mumb = len(np.where(Cities == "Mumbai")[0])
ncr = len(np.where((Cities == "New Delhi")|(Cities == "Gurgaon")|(Cities == "Noida"))[0])
```

### Plotting graph
```python
Cities = ["Bangalore" , "Mumbai" , "NCR"]
Count = [bn, mumb, ncr]
plt.bar(Cities , Count , color = ["Yellow", "Orange", "lightgreen"] , edgecolor = "black")
plt.ylabel("Number of Fundings" , fontsize = 12)
plt.xlabel("Region--------->" , fontsize = 12)
plt.title("City/Region with most number of fundings")
for i in range(len(Cities)):
    plt.text(i , Count[i]+15, Count[i] , ha ="center" ,fontsize = 10, bbox = dict(facecolor = "lime" , alpha = 0.7))
plt.show()
```
### Top 5 investors that invested in startup the most number of times
```python
name = np.array(df.InvestorsName) # converting investors name into numpy array
# Investors name has multiple name in one row separated by "commas" and " and ", by using split() we can separate the name
invt = []
for i in name:
    x = str(i).split(', ')
    for j in x:
        y = j.split(' and ')
        for k in y:
            z = k.split(',')
            for p in z:
                if 'Undisclosed' not in p:
                    invt.append(p)
np_invt = np.array(invt)   
# storing the investors name and thier frequency of investments in the dictionary as key value pairs
dct = {}
for i in np_invt:
    if i in dct:
        dct[i] = dct[i] + 1
    else:
        dct[i] = 1
#sorting dictionary value wise in descending order,  key = lambda ele:ele[1]
sort_dict = {key: val for key, val in sorted(dct.items(), key = lambda ele:ele[1], reverse = True)}
# using counter  to extract top 5 investors
k = Counter(sort_dict)
top_5 = k.most_common(5)
index = []
count = []
for i in range(5):
    index.append(top_5[i][0])
    count.append(top_5[i][1])  
```
### Plotting graph
```python
plt.bar(index , count , color = ["Orange" , "Red","Yellow", "green", "maroon"] , edgecolor = "black")
plt.xticks(rotation = 55)
plt.title("Top 5 investors")
plt.xlabel("Investors")
plt.ylabel("Number of Invesments")

for i in range(len(index)):
    plt.text(i , count[i]+1 , count[i] , ha = "center" , bbox = dict(facecolor = "lime" , alpha = 0.7))
    
plt.show()
```
### Top 5 investors to invest in most number of startups
```python
startup = np.array(df.StartupName)  #converting startup names and investor names into numpy array
invest = np.array(df.InvestorsName)
# investors names has multiple names in one rows separated by 'commas' and ' and '. By using split function
#we will separate the names and store the investors name as key and companies name as value in dct dictionary
dct = {}
for rows in range(len(invest)):
    i = str(invest[rows]).split(', ')
    
    for k in range(len(i)):
        j = i[k].split(' and ')
        
        for p in range(len(j)):
            m = j[p].split(',')
            
            for n in m:
                if n == " " or n == '' or n == 'Undisclosed Investors' or n == 'Undisclosed investors':
                    continue
                if n not in dct:
                    dct[n] = [startup[rows]]
                else:
                    dct[n].append(startup[rows])
# above dictionary contain multiple number of companies name against corresponding invrstors name. But we need
#investors name who invested in different companies. wso, we will use unique() or set() to make remove duplicate companies
# and find its number by using len().


for keys in dct:
    dct[keys] = len(set(dct[keys]))
    
#sorting dct dictionary

sort_dct = {key: val for key, val in sorted(dct.items(), key = lambda ele:ele[1], reverse = True)}
# using counter to extract top 5 investors
k = Counter(sort_dct)

top_5 = k.most_common(5)

investment_name = []        
num = []

for i in range(5):
    investment_name.append(top_5[i][0])
    num.append(top_5[i][1])
```
### Plotting graph
```python
plt.bar(investment_name , num , color = ["pink" , "yellow", "green","orange","red"] , edgecolor = "black")

plt.xticks(rotation = 45)
plt.title("Top 5 investors to invest in different companies")
plt.xlabel("Investors")
plt.ylabel("Number of Companies")
for i in range(len(investment_name)):
    plt.text(i , num[i]+1 , num[i] , ha = "center" , bbox = dict(facecolor = "lime" , alpha = 0.7))
plt.show()
```

### Top 5 investors in Crowd funding or Seed funding
```python
# Filtering the dataframe df based on the condition and making as new dataframe(temporary) as new_df
new_df = df[(df["InvestmentType"] == "Seed Funding") | (df["InvestmentType"] == "Crowd Funding")]
investor = np.array(new_df.InvestorsName)
startup = np.array(new_df.StartupName)  #converting the columns into numpy array
#investors names has multiple names in one rows separated by 'commas' and ' and '. By using split function
#we will separate the names and store the investors name as key and companies name as value in dct dictionary
dct = {}
for i in range(len(investor)):
    name = str(investor[i]).split(', ')
    for j in range(len(name)):
        name1 = name[j].split(' and ')
        for k in range(len(name1)):
            name2 = name1[k].split(',')
            for n in name2:
                if n == " " or n == "" or n == "Undisclosed Investors" or n == "Undisclosed investors":
                    continue
                if n not in dct:
                    dct[n] = [startup[i]]
                else:
                    dct[n].append(startup[i])
#above dictionary contain multiple number of companies name against corresponding invrstors name. But we need
#investors name who invested in different companies. wso, we will use unique() or set() to make remove duplicate companies
# and find its number by using len().
for keys in dct:
    dct[keys] = len(set(dct[keys]))
# sorting dictionary     
sort_dct = {key: val for key , val in sorted(dct.items() , key = lambda ele:ele[1] , reverse = True) }
k = Counter(sort_dct)
# Extracting top 5 investors using counter
top_5 = k.most_common(5)
investors = []
count = []

for i in range(5):
    investors.append(top_5[i][0])
    count.append(top_5[i][1])
```
### Plotting graph
```python
plt.bar(investors , count , color = ["green", "red" , "yellow" , "orange" , "maroon"] , edgecolor = "black")
plt.xticks(rotation = 90)
plt.xlabel("Investors")
plt.ylabel("Number of investments")
plt.title("Top 5 investors in Seed and Crowd Funding")
for i in range(len(investors)):
    plt.text(i, count[i] , count[i] , ha = "center" , bbox = dict(facecolor = "lime" , alpha = 0.6))
plt.show()
```

My Friend got his first investement by seed funding. Now he needs to go for second round of funding. His financial advisor told him that private equity would be the best investment type for the second round. So, I have to find...

### Top 5 investors in Private Equity funding InvestmentType
```python
new_df = df[(df["InvestmentType"] == "Private Equity")]  # filtering the data into new data frame as new_df
investor = np.array(new_df.InvestorsName)
startup = np.array(new_df.StartupName) #converting columns into numpy array
#investors names has multiple names in one rows separated by 'commas' and ' and '. By using split function
#we will separate the names and store the investors name as key and companies name as value in dct dictionary
dct = {}
for i in range(len(investor)):
    name = str(investor[i]).split(', ')
    for j in range(len(name)):
        name1 = name[j].split(' and ')
        for k in range(len(name1)):
            name2 = name1[k].split(',')
            for n in name2:
                if n == " " or n == "" or n == "Undisclosed Investors" or n == "Undisclosed investors":
                    continue
                if n not in dct:
                    dct[n] = [startup[i]]
                else:
                    dct[n].append(startup[i])
#above dictionary contain multiple number of companies name against corresponding invrstors name. But we need
#investors name who invested in different companies. wso, we will use unique() or set() to make remove duplicate companies
# and find its number by using len().
for keys in dct:
    dct[keys] = len(set(dct[keys]))
# sorting of dictionary    
sort_dct = {key: val for key , val in sorted(dct.items() , key = lambda ele:ele[1] , reverse = True) }

# extracting top 5 investors using counter
k = Counter(sort_dct)

top_5 = k.most_common(5)
investors = []
count = []

for i in range(5):
    investors.append(top_5[i][0])
    count.append(top_5[i][1])
```
### Plotting graph
```python
plt.bar(investors , count , color = ["aqua", "salmon" , "mediumspringgreen" , "gold" , "plum"] , edgecolor = "black")
plt.xticks(rotation = 90)
plt.xlabel("Investors")
plt.ylabel("Number of investments")
plt.title("Top 5 investors in Private Equity Funding")
for i in range(len(investors)):
    plt.text(i, count[i] , count[i] , ha = "center" , bbox = dict(facecolor = "lime"  , alpha = 0.7))
plt.show()
```

## Summary
In this analysis I analyzed top cities to establish a startup, top investors to invest in startup most number of times, top investors to invest in most number of startups and top investors in different invesment types. And I infre that NCR should be the best region for setting up the company and Indian Angel Network and Accel Partners are the wise option to seek funding for first and second rounds respectively
