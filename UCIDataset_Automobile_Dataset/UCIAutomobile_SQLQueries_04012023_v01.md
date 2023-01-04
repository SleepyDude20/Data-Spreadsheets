## Cleaning the data

### Inspecting, Identifying & Correcting Errors in Columns
#### fuel_type
There should only 2 types of values - gas & diesel

```sql
SELECT
  DISTINCT fuel_type
FROM
  cars.car_info;
```

Result: **OK**

Reason: Data falls within the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile)

#### length
Length should range from 131.1 to 208.1

```sql
SELECT
  MIN(Length) AS min_length,
  MAX(Length) AS max_length
FROM
  cars.car_info;
```

Result: **OK**

Reasom: Data falls within the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile)

#### num_of_doors
There should onlly be two values: Four or Two


```sql
SELECT
  DISTINCT num_of_doors
FROM
  cars.car_info;
```

Result: **FAIL**
Reason: There is missing data i.e. null. Upon closer inspection there are only two rows with missing data

<u>Updating missing values:</u>
Updating two rows

```sql
UPDATE
  cars.car_info
SET 
  num_of_doors = 'four'
WHERE
  make = 'dodge'
  AND fuel_type = 'gas'
  AND body_style = 'sedan';
```
```sql
UPDATE
  cars.car_info
SET 
  num_of_doors = 'four'
WHERE
  make = 'mazda'
  AND fuel_type = 'diesel'
  AND body_style = 'sedan';
```

#### num_of_cylinders
Checking to see if the data entries are error-free

```sql
SELECT
  DISTINCT num_of_cylinders
FROM
  cars.car_info;
```

Result: **FAIL**

Reason: There is an misspelling error within the values 'tow' instead-of 'two'. Looking at the dataset closer, there is only one row which has this misspell

<u>Updating errors:</u>
I'll only be updating one row:

```sql
UPDATE
  cars.car_info
SET
  num_of_cylinders = 'two'
WHERE
  num_of_cylinders = 'tow';
```

#### compression_ratio
Checking the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile) compression_ratio values should fall between 7 & 23

```sql
SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM
  cars.car_info;
```

Result: **FAIL**

Reason: Max number is 70, instead-of a max of 23. Looking into the data closely, 1 row contains a compression_ratio over 23. Checking-in with stakeholders, this row will be deleted

<u>Deleting Row:</u>
Deleting the singular erroneous row

```sql
DELETE
  cars.car_info
WHERE 
  compression_ratio = 70;
```

#### drive_wheels
Checking the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile) drive_wheels should only be: 4wd, fwd, rwd

```sql
SELECT
  DISTINCT drive_wheels
FROM
  cars.car_info;
```

Result: **FAIL**

Reason: There are two instances of 4wd when searching for unique values, therefore there is probably an extra space in one of the entries

<u>Confirming 4wd entries:</u>
Using the LENGTH() function to check the length of the entries

```sql
SELECT
  DISTINCT drive_wheels,
  LENGTH(drive_wheels) AS length
FROM
  cars.car_info;
```

Result: There is indeed an extra space within one of the entries

<u>Trimming entries:</u>
Using the TRIM() function to create consistent entries

```sql
UPDATE
  cars.car_info
SET
  drive_wheels = TRIM(drive_wheels)
WHERE 
  TRUE;
```
