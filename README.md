# 🏋️ Gym Members Exercise Data Analysis

Analisis data member gym menggunakan Python dengan tahapan **Data Cleaning**, **Exploratory Data Analysis (EDA)**, dan **Visualisasi Data**.

---

## 📖 Deskripsi

Project ini bertujuan untuk menganalisis data aktivitas member gym berdasarkan:

- Data fisik member
- Jenis workout
- Kalori yang terbakar
- BMI
- Persentase lemak tubuh
- Frekuensi latihan

Analisis dilakukan menggunakan pendekatan data science mulai dari preprocessing data hingga visualisasi insight.

---

## 🚀 Features

- Data Cleaning
- Missing Value Handling
- Duplicate Checking
- Outlier Detection
- Exploratory Data Analysis (EDA)
- Correlation Analysis
- Statistical Summary
- Data Visualization
- Insight Generation

---

## 📦 Requirements

Install dependencies terlebih dahulu:

```bash
pip install pandas numpy matplotlib seaborn missingno
```

---

## ▶️ Run Program

```bash
python main.py
```

atau jalankan notebook:

```bash
jupyter notebook
```

---

## 📁 Dataset Information

Dataset berisi informasi member gym seperti:

- Age
- Gender
- Weight
- Height
- BMI
- Workout Type
- Calories Burned
- Fat Percentage
- Water Intake
- Workout Frequency
- Experience Level

Jumlah data:

- **973 rows**
- **15 columns**

---

# 🧹 Data Cleaning

## 1. Import Library & Load Dataset

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import missingno as msno

df = pd.read_csv('/content/gym_members_exercise_tracking.csv')
```

Mengimpor library utama dan membaca dataset gym ke dalam dataframe.

---

## 2. Checking Data Structure

```python
dimensi = df.shape
tipe_data = df.dtypes
sample_data = df.head()

(dimensi, tipe_data, sample_data)
```

Digunakan untuk melihat:

- Jumlah baris dan kolom
- Tipe data tiap kolom
- Contoh isi dataset

---

## 3. Unique Value Analysis

```python
unique_gender = df['Gender'].unique()
unique_workout_type = df['Workout_Type'].unique()
unique_experience_level = df['Experience_Level'].unique()

(unique_gender, unique_workout_type, unique_experience_level)
```

Mengecek variasi data kategorikal pada:

- Gender
- Workout Type
- Experience Level

---

## 4. Missing Value Handling

```python
missing_values = df.isnull().sum()

msno.matrix(df)
plt.show()

df = df.dropna()
```

Tahapan:

- Menghitung missing value
- Visualisasi missing value
- Menghapus data kosong

---

## 5. Duplicate Checking

```python
jumlah_duplikat = df.duplicated().sum()

df = df.drop_duplicates()

jumlah_duplikat_baru = df.duplicated().sum()

print("Jumlah duplikat sebelum penghapusan:", jumlah_duplikat)
print("Jumlah duplikat setelah penghapusan:", jumlah_duplikat_baru)
```

Digunakan untuk memastikan tidak ada data duplikat.

---

## 6. Outlier Detection

```python
fig, axs = plt.subplots(1, 2, figsize=(12,5))

sns.boxplot(y=df['Calories_Burned'], ax=axs[0])
sns.boxplot(y=df['BMI'], ax=axs[1])

plt.tight_layout()
plt.show()
```

Visualisasi outlier menggunakan boxplot.

---

## 7. Reset Index

```python
df = df.reset_index(drop=True)
```

Mereset index setelah proses cleaning selesai.

---

# 📊 Exploratory Data Analysis (EDA)

## 1. Descriptive Statistics

```python
statistik_deskriptif = df.describe(include='all')
statistik_deskriptif
```

Menampilkan statistik deskriptif dataset.

---

## 2. Categorical Data Analysis

```python
fig, axs = plt.subplots(1, 3, figsize=(18,5))

sns.countplot(x='Gender', data=df, ax=axs[0])
axs[0].set_title('Distribusi Gender')

sns.countplot(x='Workout_Type', data=df, ax=axs[1])
axs[1].set_title('Distribusi Workout Type')

sns.countplot(x='Experience_Level', data=df, ax=axs[2])
axs[2].set_title('Distribusi Experience Level')

plt.tight_layout()
plt.show()
```

Menganalisis distribusi kategori data.

---

## 3. Correlation Analysis

```python
plt.figure(figsize=(12, 8))

sns.heatmap(df.corr(numeric_only=True),
            annot=True,
            cmap='YlGnBu')

plt.title('Matriks Korelasi Antar Fitur Numerik')
plt.show()
```

Digunakan untuk melihat hubungan antar variabel numerik.

### Insight

- Calories_Burned memiliki korelasi tinggi dengan Session_Duration
- BMI berkorelasi kuat dengan Weight
- Workout_Frequency berkorelasi positif dengan Experience_Level
- Fat_Percentage berkorelasi negatif dengan Workout_Frequency

---

## 4. Outlier Analysis After Cleaning

```python
def hitung_outlier(series):
    q1 = series.quantile(0.25)
    q3 = series.quantile(0.75)

    iqr = q3 - q1

    bawah = q1 - 1.5 * iqr
    atas = q3 + 1.5 * iqr

    return ((series < bawah) | (series > atas)).sum()

outlier_calories = hitung_outlier(df['Calories_Burned'])
outlier_bmi = hitung_outlier(df['BMI'])

(outlier_calories, outlier_bmi)
```

Menghitung jumlah outlier pada:

- Calories Burned
- BMI

---

# 📈 Data Visualization

## 1. Distribution of Age & BMI

```python
fig, axs = plt.subplots(1, 2, figsize=(12, 5))

sns.histplot(df['Age'], bins=20, kde=True, ax=axs[0])
axs[0].set_title('Distribusi Usia Member')

sns.histplot(df['BMI'], bins=20, kde=True, ax=axs[1])
axs[1].set_title('Distribusi BMI Member')

plt.show()
```

Menampilkan distribusi usia dan BMI member gym.

---

## 2. Workout Type by Gender

```python
plt.figure(figsize=(8,5))

sns.countplot(x='Workout_Type',
              data=df,
              hue='Gender')

plt.title('Workout Type Berdasarkan Gender')
plt.show()
```

Membandingkan jenis workout berdasarkan gender.

---

## 3. BMI Boxplot by Gender

```python
plt.figure(figsize=(7,5))

sns.boxplot(x='Gender',
            y='BMI',
            data=df)

plt.title('Perbandingan BMI berdasarkan Gender')
plt.show()
```

Menampilkan distribusi BMI berdasarkan gender.

---

## 4. Calories Burned per Workout Type

```python
plt.figure(figsize=(10,6))

sns.boxplot(x='Workout_Type',
            y='Calories_Burned',
            data=df)

plt.title('Calories Burned per Workout Type')
plt.show()
```

Menganalisis kalori terbakar berdasarkan jenis workout.

---

## 5. Fat Percentage vs BMI

```python
plt.figure(figsize=(8,6))

sns.scatterplot(x='BMI',
                y='Fat_Percentage',
                hue='Gender',
                data=df)

plt.title('Fat Percentage vs BMI')
plt.show()
```

Melihat hubungan antara BMI dan persentase lemak tubuh.

---

## 6. Average Calories Burned by Experience Level

```python
level_calories = df.groupby('Experience_Level')['Calories_Burned'].mean()

plt.figure(figsize=(8,5))

level_calories.plot(kind='bar')

plt.title('Rata-rata Calories Burned per Experience Level')
plt.ylabel('Rata-rata Kalori Terbakar')

plt.show()
```

Menampilkan rata-rata kalori terbakar berdasarkan level pengalaman.

---

## 7. Gender Composition

```python
plt.figure(figsize=(6,6))

gender_counts = df['Gender'].value_counts()

plt.pie(gender_counts,
        labels=gender_counts.index,
        autopct='%1.1f%%',
        startangle=140,
        colors=['#6ca0dc','#ffb347'])

plt.title('Komposisi Gender Member Gym')

plt.axis('equal')
plt.show()
```

Visualisasi komposisi gender member gym.

---

# 🛠️ Built With

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Missingno

---

# 🎯 Purpose

Project ini dibuat untuk:

- Pembelajaran Data Analysis
- Exploratory Data Analysis (EDA)
- Data Cleaning
- Visualisasi Data
- Analisis data fitness dan kesehatan
