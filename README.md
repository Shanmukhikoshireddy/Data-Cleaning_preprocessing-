# 🩺 Medical Appointment No-Show Dataset – Data Cleaning & Preprocessing
This project focuses on cleaning and preprocessing the Medical Appointment No-Show dataset. The primary goal of this stage is to ensure data integrity by removing null, duplicate records  and convert string-based date columns into appropriate datetime formats and standardize text values and column names for consistency 


**Dataset Source**: [Kaggle – No-show Appointments](https://www.kaggle.com/datasets/joniarroba/noshowappointments)  
The dataset includes features such as ```PatientID, Appointment Date, Scheduled Date, Age, Gender, and medical conditions like Diabetes and Hypertension```. The key target column is No-show, indicating whether the patient missed the appointment.\
**Total Records**: `110,527` rows and `14` columns  
**Objective**: Clean and preprocess the dataset for further analysis or model building.

---

## 🧰 Modules Used

| Module      | Purpose                                                         |
|-------------|-----------------------------------------------------------------|
| `pandas`    | Reading CSV, manipulating DataFrame                             |
| `datetime`  | Handling date/time conversions                                  |
| `missingno` | Visualizing missing/null values                                 |
| `matplotlib`| Plotting missing data graphs                                    |

---

## 🧼 Data Cleaning & Preprocessing Steps

### 1. **Load the Dataset**

```python
df = pd.read_csv("Medical_appointment.csv")
```

---

### 2. **Initial Exploration**

```python
df.shape
```
- Returns the shape of the dataset: `(110527, 14)`

```python
df.describe(include="number").T
```
- Summary statistics for numeric columns (count, mean, std, min, quartiles, max)

```python
df.describe(include="O").T
```
- Summary for object (string-like) columns (count, unique values, top, frequency)

```python
df.info()
df.dtypes
```
- Overview of column datatypes and null values

---

### 3. **Datetime Conversion**

```python
df['ScheduledDay'] = pd.to_datetime(df['ScheduledDay'], errors='coerce')
df['ScheduledDate'] = df['ScheduledDay'].dt.date
df['ScheduledTime'] = df['ScheduledDay'].dt.time
df = df.drop(columns=['ScheduledDay'])
```

```python
df['AppointmentDay'] = pd.to_datetime(df['AppointmentDay'], errors='coerce')
df['AppointmentDate'] = df['AppointmentDay'].dt.date
df['AppointmentTime'] = df['AppointmentDay'].dt.time
df = df.drop(columns=['AppointmentDay'])
```

- Converted datetime columns and extracted `date` and `time` into separate columns for flexibility.

---

### 4. **Handling Missing Values**

```python
df.isnull().sum()
```
- Shows the count of null values per column.

```python
import missingno as msno
import matplotlib.pyplot as plt

msno.matrix(df)
plt.title("Null Value Graph", fontsize=20, weight="bold")
plt.show()
```
- Visual representation of nulls using `missingno`.

```python
df = df.dropna()
```
- Removed rows with missing values.

---

### 5. **Remove Duplicate Rows**

```python
df.duplicated().sum()
df = df.drop_duplicates()
```

---

### 6. **Standardize Column Names**

```python
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')
```
- Ensures uniform and clean column headers.

---

### 7. **Standardize Categorical Text**

```python
df['gender'] = df['gender'].str.upper().str.strip()
df['no-show'] = df['no-show'].str.upper().str.strip()
```

- Example: converting `f`, `F`, ` female ` → `F`

```python
df['gender'].nunique()
df['no-show'].nunique()
```
- Check uniqueness after standardization.

---

### 8. **Fix Column Data Types**

```python
df = df.astype({
    'patientid': 'str',
    'appointmentid': 'str',
    'gender': 'category',
    'age': 'int',
    'scholarship': 'int',
    'hipertension': 'int',
    'diabetes': 'int',
    'alcoholism': 'int',
    'handcap': 'int',
    'sms_received': 'int',
    'no-show': 'category',
    'appointmentdate': 'datetime64[ns]',
    'scheduleddate': 'datetime64[ns]',
})
```
- Ensures data consistency for analysis or modeling.

---

### 9. **Save Cleaned Data**

```python
df.to_excel('output.xlsx', index=False)
```
- Final cleaned dataset saved as `output.xlsx`

---

This cleaned dataset will serve as a reliable foundation for understanding patient behavior and predicting appointment no-shows, which can help improve scheduling efficiency in healthcare systems.


