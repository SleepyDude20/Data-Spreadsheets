## Cleaning the data

### Inspecting Columns
#### fuel_type
There should only 2 types of values - gas & diesel

```sql
SELECT
  DISTINCT fuel_type
FROM
  cars.car_info;
```

Result: **OK** - Data falls within the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile)

#### length
Length should range from 131.1 to 208.1

```sql
SELECT
  MIN(Length) AS min_length,
  MAX(Length) AS max_length
FROM
  cars.car_info;
```

Result: **OK** - Data falls within the [data description](https://archive.ics.uci.edu/ml/datasets/Automobile)

#### num_of_doors
There should onlly be two values: Four or Two


```sql
SELECT
  DISTINCT num_of_doors
FROM
  cars.car_info;
```

Result: **FAIL** - There is missing data i.e. null. Upon closer inspection there are only two rows with missing data

<u>Updating missing values:</u>
Updating two rows:

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
