---
description: >-
  User guideline for Clara's program - empirical assessment of database
  configuration parameters under a given workload.
---

# README.md

## What Need to Be Done Before Start?

1. Pick RDBMS
2. Pick a representative dataset
3. Figure out the representative workload with picked dataset



## Load Config File 

Export\(or copy and paste\) config file from the application that you want to test. For example, to test PostgreSQL, copy parameter name, setting, minimum value and maximum value from table pg\_settings with following SQL command:

```sql
select name, setting, min_val, max_val from pg_settings;
```

While using function readfile in the program, put the name of config file in single quotes. 

```
 readfile('numerical_config.csv.small')
```

{% hint style="info" %}
Put exported config file in the same folder with the program. 
{% endhint %}

## Run Test

Create an empty table with needed column names and data type before loading data into database. The SQL command used to create an empty table is in the file createtable.sql under folder preparation. 

Insert the SQL commands that you think can represent the complexity of work\(aka, "representative workload" \) into the function workload. 

Change the content in cur.execute\(\) into desired commands.   

```python
def workload(file,table_name):

    print("load the data into the db")
    #params_name =               ["tcp_keepalives_count","cursors"]
    with open('./'+ file, 'r') as f:
        reader = csv.reader(f)
        next(reader)  # Skip the header row.
        for row in reader:
            cur.execute( 'INSERT INTO '+ table_name +' VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)',
                        row
                       )
    #sql statement
    print("run the select")
    cur.execute("select * from sof a, sof b, sof c where not a.undergradmajor ~ '[mM]ath' and not b.hopefiveyears ~ '[Ww]ork' and not c.hopefiveyears ~ 'calender' and not a.devtype ~ '[fF]ull-stack' and not b.country ~ 'Kenya';")
    return

```

{% hint style="info" %}
Also, % symbol in for loop corresponding to how many columns you have in the selected dataset. For example, in the dataset I selected, there were 26 columns, so there are 26 % symbols in VALUES\(\) function. Don't forget to change that if you have more than 26 columns. 
{% endhint %}

## Analysis 

### Descriptive Graphs  

