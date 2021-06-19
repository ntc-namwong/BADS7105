# Analysis of Customer Behavior

This is a survey in BADS7105 class asking students the interests and consumptions for serveral activites, such as food, cosmetic, game and so on. Even the sample size is not large enough to represent the population in Thailand or in the world, I can get some insights which will be described below.

Below will descibed how to pre-process and analyze the customer behavior data by using Python.

## Data Preparation

First of all, let's start with importing essential libraries.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

Before pre-processing, it is neccessary to know the data first. Since I know that the first column is response timestamp which is meaningless in this analysis, so the first column is neglect.

```python
df = pd.read_excel('Customer Behaviors (Responses).xlsx')
df = df[df.columns[1:]]
```

It is obvious that there are long texts for both column name and response data. For more clear understanding, these long texts are replaced.

```python
# rename columns
intr_col = [c.replace('คุณมีความสนใจในสิ่งเหล่านี้มากน้อยเพียงใด', 'Interest') for c in df.columns if 'คุณมีความสนใจในสิ่งเหล่านี้มากน้อยเพียงใด' in c]
cons_col = [c.replace('คุณบริโภคสิ่งเหล่านี้บ่อยขนาดไหน', 'Consumption') for c in df.columns if 'คุณบริโภคสิ่งเหล่านี้บ่อยขนาดไหน' in c]
df.columns = intr_col + cons_col + ['Others', 'Date of Birth', 'Gender']

# categorize data
df[intr_col] = df[intr_col].replace({'ไม่สนใจอย่างมากที่สุด':1, 'ไม่สนใจอย่างมาก':2, 'ไม่สนใจ':3, 'เฉยๆ':4,
                                     'สนใจ':5, 'สนใจอย่างมาก':6, 'สนใจอย่างมากที่สุด':7})
df[cons_col] = df[cons_col].replace({'แทบไม่ได้บริโภคเลย':1, 'หลายเดือนครั้ง':2, 'เดือนละ 2-3 ครั้ง':3,
                                     'เดือนละครั้ง':4, 'อาทิตย์ละครั้ง':5, 'แทบทุกวัน':6})
df['Gender'] = df['Gender'].map({'ชาย':'M', 'หญิง':'F', 'ไม่ต้องการระบุ':np.nan})
```

Moreover, some date of birth are in Buddhist calendar (B.E.), some are in Anno Domini (A.D.). For more consistency and easy to analysis, it is required to convert those into the same basis which A.D. is chosen becuase it is default format for pandas library.

I found an issue when using to_datetime function to convert the calendar year because the maximum datetime for this function is at April 11<sup>th</sup>, 2262. Therefore, what I have done is to split date of birth into 3 columns, including year (Y), month (M) and date (D). After apply a calculation to convert B.E. to A.D., all 3 columns are concatenated together again and finally converted to datetime format using to_datetime function.

```python
df['Y'] = df['Date of Birth'].apply(lambda x: int(str(x).split()[0].split('-')[0])).apply(lambda x: x if x < 2500 else x - 543)
df['M'] = df['Date of Birth'].apply(lambda x: int(str(x).split()[0].split('-')[1]))
df['D'] = df['Date of Birth'].apply(lambda x: int(str(x).split()[0].split('-')[2]))

df['Date of Birth'] = pd.to_datetime(df['Y'].astype(str) + '-'+ df['M'].astype(str) + '-' + df['D'].astype(str))
```

This study plans to use 3 attributes, including gender, age and zodiac, for finding correlation and seeing the insights. Thus, age and zodiac have to be established.

```python
df['Age'] = df['Date of Birth'].dt.year.apply(lambda x: 2021 - x if x < 2020 else np.nan)
```

```python
def zodiac(df):
    if (df['D'] >= 15 and df['M'] == 1) or (df['D'] < 13 and df['M'] == 2):
        return 'Capricon'        
    elif (df['D'] >= 13 and df['M'] == 2) or (df['D'] < 15 and df['M'] == 3):
        return 'Aquarius'
    elif (df['D'] >= 15 and df['M'] == 3) or (df['D'] < 13 and df['M'] == 4):
        return 'Pisces'
    elif (df['D'] >= 13 and df['M'] == 4) or (df['D'] < 15 and df['M'] == 5):
        return 'Aries'
    elif (df['D'] >= 15 and df['M'] == 5) or (df['D'] < 15 and df['M'] == 6):
        return 'Taurus'
    elif (df['D'] >= 15 and df['M'] == 6) or (df['D'] < 15 and df['M'] == 7):
        return 'Gemini'
    elif (df['D'] >= 15 and df['M'] == 7) or (df['D'] < 16 and df['M'] == 8):
        return 'Cancer'
    elif (df['D'] >= 16 and df['M'] == 8) or (df['D'] < 17 and df['M'] == 9):
        return 'Leo'
    elif (df['D'] >= 17 and df['M'] == 9) or (df['D'] < 17 and df['M'] == 10):
        return 'Virgo'
    elif (df['D'] >= 17 and df['M'] == 10) or (df['D'] < 16 and df['M'] == 11):
        return 'Libra'
    elif (df['D'] >= 16 and df['M'] == 11) or (df['D'] < 16 and df['M'] == 12):
        return 'Scorpio'
    elif (df['D'] >= 16 and df['M'] == 12) or (df['D'] < 15 and df['M'] == 1):
        return 'Sagittarius'

df['Zodiac'] = df.apply(zodiac, axis = 1)
```

Last but not least, unused columns are dropped before proceed next step.

```python
df.drop(['Y', 'M', 'D'], axis = 1, inplace = True)
df.dropna(inplace = True)
```

The result of all above steps is pivot-type table. Unfortunately, some of visualizations requires unpivot-type table. So, few steps below are used to prepare unpivot table.

```python
survey_index = dict(zip(intr_col + cons_col, range(len(intr_col + cons_col))))

df_unpivoted = df[intr_col + cons_col + ['Gender', 'Age', 'Zodiac']].melt(id_vars = ['Gender', 'Age', 'Zodiac'], var_name = 'Survey', value_name = 'Score')
df_unpivoted['Survey Index'] = df_unpivoted['Survey'].map(survey_index)
df_unpivoted['Zodiac Index'] = df_unpivoted['Zodiac'].map(zodiac_index)
df_unpivoted.sort_values(['Survey Index', 'Zodiac Index'], ascending = [True, True], inplace = True)
df_unpivoted.drop(['Survey Index', 'Zodiac Index'], axis = 1, inplace = True)
```

## EDA

### Demographic

Demographics of surveyees are shown in picture below. It can be seen that BADS7105 female students are around half of all students. Most of all student are between 25-30 years old. Moreover, some of zodiacs shows imbalance distribution such as Aquarius, Taurus, Pisces, Libra, Leo and Gemini.

![Picture 1-1](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2001/Picture%201-1%20Demographic.jpg)

To create above visualization, it can be done by using below codes.

```python
fig, (ax1, ax2, ax3) = plt.subplots(1, 3, figsize = (15, 4))
```

```python
# Demographic by Gender
ax1.set_title('Demographic by Gender')
ax1.bar(df['Gender'].unique(), df['Gender'].value_counts(), color = ['tab:blue', 'lightpink'])
ax1.set_xlabel('Gender')
ax1.set_ylabel('Count')
```

```python
# Demographic by Age
ax2.set_title('Demographic by Age')
ax2.hist([df[df['Gender'] == 'M']['Age'], df[df['Gender'] == 'F']['Age']], bins = 15, rwidth = 0.9, stacked = True, color = ['tab:blue', 'lightpink'])
ax2.yaxis.set_major_locator(MaxNLocator(integer = True))
ax2.set_xlabel('Age')
ax2.set_ylabel('Frequency')
```

```python
# Demographic by Zodiac
ax3.set_title('Demographic by Zodiac')
df_zodiac = pd.DataFrame([[x, y, df[df['Zodiac'] == x][df['Gender'] == y]['Age'].count()] for _, x in enumerate(df['Zodiac'].unique()) for _, y in enumerate(df[df['Zodiac'] == x]['Gender'].unique())], columns = ['Zodiac','Gender','Count']).sort_values(['Zodiac', 'Gender'])
ax3.pie(df_zodiac['Count'], colors = ['lightpink', 'tab:blue'] * 10, radius = 0.7, wedgeprops = dict(width = 0.05), startangle = 90)
ax3.set_prop_cycle('color', [plt.get_cmap('bone')(x / len(df_zodiac['Zodiac'].unique())) for x, _ in enumerate(df_zodiac['Zodiac'].unique())])
_, _, autotexts = ax3.pie(df_zodiac.groupby(['Zodiac']).sum()['Count'], labels = df_zodiac['Zodiac'].unique(), wedgeprops = dict(width = 0.3), startangle = 90, autopct = lambda x: '{:,.0f}'.format(x * df_zodiac['Count'].sum() / 100), pctdistance = 0.85)
[autotext.set_color('white') for autotext in autotexts]
```

```python
plt.show()
```

