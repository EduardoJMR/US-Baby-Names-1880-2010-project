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
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/Capture.JPG)

#### Since we have tracked the number of births by year (births= Sum of children with each name), we can show this data by gender.

```python
total_births= names.pivot_table("births", index="year", columns="sex", aggfunc=sum)
total_births.plot(title="Total births by sex and year")
```
![image](https://github.com/EduardoJMR/US-Baby-Names-1880-2010-project/blob/master/Capture2.JPG)












