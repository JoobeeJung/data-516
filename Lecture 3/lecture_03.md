---
marp: true
theme: default
paginate: true
header: "UW CSED516 Fall 2024 - Scalable Data Systems and Algorithms - Professor Mark Kazzaz"
---

# Housekeeping
- Use your provided UW ID for assignments
  - It needs to match the single-sign on between UW NetID systems and Canvas
  - Not your custom email alias
---

# Updated course materials
- Updated syllabus and course repor
  - Contains updated schedule
  - Homework1 now published
---

# Communication
- Leaving Slack
  - Let's migrate to UW's instance of Mattermost
    - https://mattermost.cs.washington.edu/signup_user_complete/?id=czthsr8qztnmxjbgdtokn346ze&md=link&sbr=su
    - Requires you to log into UW's Gitlab instance.
- Mattermost is still not an official communication channel
  - Necessary for sharing code snippets during class without risk of including special characters
  - Good place to work together as a class to post questions/code
  - Required to post a question here first before attending office hours
- Official communication needs to be sent via email or Canvas
  - eg: running laet to class, can't attend class, questions about syllabus
---

# Using EC2 for data / data science work

- launch ec2 compute instance
  - manually via web
  -  https://medium.com/@bobbycxy/detailed-guide-to-connect-ec2-with-vscode-2c084c265e36
     -  Great reference but we can't follow it 100%

---

---

# Introduction to Polars: A Fast DataFrame Library
A high-performance DataFrame library designed for fast and efficient data processing.

Best resource for learning: https://docs.pola.rs/

---

# Why Choose Polars?
- **Performance**: Handles large datasets efficiently using multi-threading.
- **Memory usage**: Optimized for working on systems with limited memory.

---
- **Differences from Pandas**:
  - Polars is designed from the ground up for speed.
  - Polars is natively multi-threaded, which allows it to utilize multiple cores during operations, unlike Pandas which is single-threaded.
  - Polars uses Arrow for memory efficiency, making it better suited for out-of-core data processing.

---

# **Where Pandas excels**:
- Better ecosystem support and wider adoption.
- More extensive and mature support for missing data.
- More extensive built-in methods and more flexibility with custom functions.

---

# Core Features of Polars
- **DataFrames**: Like Pandas but optimized for performance.
- **Expressions**: Use concise queries and operations on DataFrames.
- **Out-of-Core Operations**: Polars can process data that doesn't fit into memory efficiently.

---

# Working with Polars DataFrames
## Creating a DataFrame
```python
import polars as pl
df = pl.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "salary": [50000, 60000, 70000]
})
df
```
- Polars DataFrame creation is similar to Pandas, but faster!


---
# Creating DataFrames from Excel and CSV Files

### From a CSV file:
```python
import polars as pl

# Read a CSV file
df_csv = pl.read_csv("data.csv")
print(df_csv)

# Selecting Columns in Polars
df.select("name", "age")
```

- You can select specific columns by passing the column names.

---

# Filtering Rows in Polars
```python
df.filter(pl.col("age") > 30)
```
- Filter rows based on conditions, similar to Pandas.
- Intuitive verbs make it easy to read and understand each operation.


---

# Group By and Aggregate in Polars
```python
df.groupby("age").agg([
    pl.col("salary").mean().alias("avg_salary")
])
```
- Group by a column and apply aggregate functions (e.g., mean, sum).


---

# Adding or Modifying Columns with `with_columns`
```python
df.with_columns([
    (pl.col("salary") * 1.05).alias("salary_with_raise")
])
```
- Use `with_columns` to add new columns or modify existing ones.
- Efficient handling of new columns without unnecessary copying.

---

# Dropping Columns
```python
df.drop("salary")
```
- Dropping columns is straightforward and optimized in Polars.
- Reduces memory overhead compared to Pandas.

---

# Renaming Columns
```python
df.rename({"name": "employee_name"})
```
- Rename columns using a dictionary to map old names to new names.
- Convenient for preparing data for further analysis.

---

# Unpivot in Polars
```python
df.unpivot(index="Item")
```
- Reshape data from wide to long format.
- Similar to `pd.melt()`, but faster on large datasets.
- `df.pivot` reshapes data from long to wide.

---

# Joining DataFrames
```python
df.join(other_df, on="name", how="left")
```
- Polars supports multiple types of joins (left, right, inner, etc.), like Pandas.

---

# Sorting Data in Polars
```python
df.sort("salary", descending=True)
```
- Sort data based on a specific column.
- Multi-threaded sorting for faster performance.

---
# Chaining Methods in Polars

```python
import polars as pl

# Read from CSV, select columns, create new columns, aggregate, and filter
df = (
    pl.read_csv("data.csv")
    .select(["name", "age", "salary"])
    .with_columns(
        [
          (pl.col("salary") * 1.1).alias("salary_with_bonus"),
          (pl.col("age") + 5).alias("age_in_5_years")
        ]
      )
    .groupby("name")
    .agg(
      [
        pl.col("salary_with_bonus").mean().alias("avg_salary_with_bonus")
      ]
    )
    .filter(pl.col("avg_salary_with_bonus") > 50000)
)
```
---

# Conclusion
- **Polars** offers a high-performance alternative to Pandas for handling large-scale data.
- Faster for select, filter, group-by, and other common operations.
- While Pandas is more flexible in some areas, Polars excels in performance.