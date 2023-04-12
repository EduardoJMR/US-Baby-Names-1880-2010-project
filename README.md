# US-Baby-Names-1880-2010-project

#### We have here a database broken down into a number of txt files, one for each year from 1880 to 2010. Each of the files contains the name, the sex, and the number of occurrences of the name.

#### National Data on the relative frequency of given names in the population of U.S. births where the individual has a Social Security Number (Tabulated based on Social Security records as of March 6, 2022). For each year of birth YYYY after 1879, we created a comma-delimited file called yobYYYY.txt. Each record in the individual annual files has the format "name,sex,number," where name is 2 to 15 characters, sex is M (male) or F (female) and "number" is the number of occurrences of the name. Each file is sorted first on sex and then on number of occurrences in descending order. When there is a tie on the number of occurrences, names are listed in alphabetical order. This sorting makes it easy to determine a name's rank. The first record for each sex has rank 1, the second record for each sex has rank 2, and so forth. To safeguard privacy, we restrict our list of names to those with at least 5 occurrences.

### Extracting data

#### The first thing we are going to need to do is merge all the files together. Adding a column with the year every data corresponds in every file.

```python
pieces= []
for year in range(1880,2011):
    path= f"babynames/yob{year}.txt"
    frame= pd.read_csv(path, names=["name", "sex", "births"])
    
    #Add a column for the year
    frame["year"]=year
    pieces.append(frame)

#Concatenate everything into a single DataFrame
names=pd.concat(pieces, ignore_index=True)
names
```
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/images/Capture.JPG)

#### Since we have tracked the number of births by year (births= Sum of children with each name), we can show this data by gender.

```python
total_births= names.pivot_table("births", index="year", columns="sex", aggfunc=sum)
total_births.plot(title="Total births by sex and year")
```
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/images/Capture2.JPG)

## Female Naming Trends?

### Transforming the data

#### Once we have the number of births by name we can get the proportion of babies given each name relative to the total number of births by year and sex

```python
def add_prop(group):
    group["prop"]= group["births"]/group["births"].sum()
    return group

names=names.groupby(["year","sex"]).apply(add_prop)
names
```
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/images/Capture3.JPG)

#### With the proportion of every name we can study which are the names used throughout the time by gender. So, we are getting 1000 more chosen names.

```python
def get_top1000(group):
    return group.sort_values("births", ascending=False)[:1000]
grouped= names.groupby(["year", "sex"])
top1000=grouped.apply(get_top1000)

top1000= top1000.reset_index(drop=True)
```

#### Female Naming Trends

```python
girls= top1000[top1000["sex"] == "F"]
```
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/images/Capture4.JPG)

## A few boy and girl names over time

### Transforming the data

#### We can study how many times the proportion of any name has changed throughout time. To know that we need to change the data frame distribution.

```python
total_births= top1000.pivot_table("births", index="year", columns="name", aggfunc=sum)
subset=total_births[["John", "Harry", "Mary", "Marilyn"]]
subset.plot(subplots=True, figsize=(12,10), title="Number of births per year")
```

### Visualizing the data

![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/images/Capture5.JPG)


