# Value Proposition

Based on a day in life data from BADS7105 students, activity for each person always changes along the day. Visualization below summarizes BADS7105 students activities as well as gain and pain points.

## Result Summary

![Picture 3-1](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2003/Picture%203-1%20Activities%20in%20a%20Day%20Life.gif)

> 00:00 AM - 05:30 AM : Sleeeping
- ***Context***
  - After the long day of yesterday, most of students are sleeping.
- ***Gain***
  - Body is recovering.
  - Energy is boosted.
  - It feels peaceful.
- ***Pain***
  - Someone feels worried about tomorrow.
  - Someone feels worried about homework.
  - Someone want to see more series.

> 05:30 AM - 08:00 AM : Sleeeping
- ***Context***
  - Majority still be sleeping but alarm clock start ringing for somebody.
- ***Gain***
  - Body is recovering.
  - Energy is boosted.
  - It feels peaceful.
- ***Pain***
  - It feels very sleepy.
  - It is lazy getting up from the bed.

> 08:00 AM - 08:30 AM : Traveling
- ***Context***
  - There are serveral types of traveling. Someone go to work by personal car, others go by public transportation such as BTS or bus.
- ***Gain***
  - Something can be done on the way whether listen to music, watch NetFlix, have breakfast or play mobile phone.
  - Working at office is more productive than working from home.
  - By using personal car, it reduces contact with people.
  - By using public transportation, time can be managed.
- ***Pain***
  - It is traffic jam, espectially in Bangkok.
  - It is required to carry laptop and device around everyday.
  - Workplace is far from home or condo.

> 08:30 AM - 12:00 PM : Work
- ***Context***
  - In the first half day of work, most of people still have a fresh brain in the early morning.
- ***Gain***
  - Car parking is still available.
  - Work is prioritized.
  - The work is progressing.
  - The work can be completed as plan.
- ***Pain***
  - There are a lot of unimportant email.
  - It feels stress when notice about upcoming work.
  - It feels lazy and boring.
  - Internet is unstable leading to an obstacle in call, meeting and conference.
  - There are an urgent task which has to be finshed within a day.

> 12:00 PM - 01:00 PM : Lunch
- ***Context***
  - Lunch time, it is nothing special.
- ***Gain***
  - It is full, recieved enough energy to continue the work in the afternoon.
  - It is relexing time after working.
  - It is good opportunity to change atmosphere.
- ***Pain***
  - Food is sold out too early.
  - It is hard to decide what will we eat this lunch.
  - Sometime, restaurant is far from office.

> 01:00 PM - 06:00 PM : Work
- ***Context***
  - Even energy is charged from luch, but it seems to be too much.
  - It is observed that more than half still be working overtime.
- ***Gain***
  - The work is progressing.
  - The work can be completed as plan.
  - It can learn something new to gain more knowledge.
- ***Pain***
  - It feels sleepy because of too much full.
  - New issues come out everyday.

> 06:00 PM - 08:15 PM : Dinner
- ***Context***
  - Most students have dinner longer than lunch. It trends to fat from dinner.
- ***Gain***
  - In dinner conversation, it can get some insight from some colleagues.
  - It is full after meal.
- ***Pain***
  - Healthy food comes up with undesirable taste.
  - Some kind of foods is smelly to hands and hair.
  - Again, it is hard to decide what will we eat this dinner.

> 08:15 PM - 08:45 PM : Traveling
- ***Context***
  - Less people complain about traffic jam. This might be because we can manage to go back home depend on dinner.
- ***Gain***
  - Traffic is not jam.
- ***Pain***
  - It feels sleepy.

> 08:45 PM - 00:00 AM : Education
- ***Context***
  - Most of students are always willing to learn new things. Someone reads book, someone does homework. However, somebody go to the bed since 10:00 PM.
- ***Gain***
  - Homework has been finished.
  - It can gain more knowledge.
- ***Pain***
  - Sometime, homework make us dispondent.
  - It is very lazy, but homework has still not finished.

## Step-by-step Approach

In this study, there are 4 main steps, including survey combination, labeling, data preparation, and finally visualization. All steps are done by 3 softwares below:
- Google Colab [![Open In Collab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ntc-namwong/BADS7105/blob/main/Homework%2003/Homework%203.ipynb) 
- Microsoft Excel [![](https://img.shields.io/badge/-Link%20MS%20Excel-blue)](https://github.com/ntc-namwong/BADS7105/tree/main/Homework%2003/Activities.xlsx)
- Microsoft Power BI [![](https://img.shields.io/badge/-Link%20MS%20Power%20BI-blue)](https://github.com/ntc-namwong/BADS7105/tree/main/Homework%2003/Homework%203.pbix)

### Survey Combination

Due to the BADS7105 students' activities survey raw data have been uploaded separately, it is required to sum up all the files together in order to easy to proceed in next step.

In this step, python coding in Colab has been used. By using glob library to browse Activities files, the Excel files are read by using library pandas and appended to new dataframe.

```python
import glob
import pandas as pd

df = pd.DataFrame()

for f in glob.glob('Raw Data/Activities_*xlsx'):
    data = pd.read_excel(f)
    data['File'] = f
    df = df.append(data, ignore_index = True)

for f in glob.glob('Raw Data/Activities_*xls'):
    data = pd.read_excel(f)
    data['File'] = f
    df = df.append(data, ignore_index = True)

df.to_excel('Activites.xlsx')
```

### Labeling

Purpose of this step is to label activities with some keywords. This is because the detail activites are diverse but they can be grouped into some keywords. For example, whether `ทำงาน`, `เริ่มงาน` or `ทำงานต่อ`, all of these can be described be wording `Work`.

As of now, this step has been done manually in Microsoft Excel. This is room to improvement for this study because there might be some library in python to do text analytics and grouping long words into key words.

### Data Preparation

In this step, python coding is applied to create 4 tables using in Power BI.

> Scatter Plot Coordination Table

On the right handside of the dashboard, it is designed to be circle which contains multiple points of activities on the circumference and traveling activity on the center. Trigonometry is easiest way to create the coordinate on the circumference. Sine function represents y-axis and cosine function represents x-axis.

From activities data, the unque activity has been looped to generate coordinate around the circumference. Finally, traveling activity is appended to the dataframe with coordinate (0,0) because it is normal to move from one activity to another activity by traveling.

```python
import numpy as np
import pandas as pd

data = []
df = pd.read_excel('Activities.xlsx')

r = 5
X = lambda i, a: 0 if a == 'Traveling' else round(r*np.cos(np.pi/2 - i*(2*np.pi)/(len(df['Label'].unique())-1)), 3)
Y = lambda i, a: 0 if a == 'Traveling' else round(r*np.sin(np.pi/2 - i*(2*np.pi)/(len(df['Label'].unique())-1)), 3)

for i, a in enumerate(np.sort(np.delete(df['Label'].unique(), np.where(df['Label'].unique() == 'Traveling'), axis=0))):
    data.append([a, X(i, a), Y(i, a)])

data.append(['Traveling', 0, 0])    
df_coordinate = pd.DataFrame(data, columns = ['Activity', 'X', 'Y']).set_index('Activity')

df_coordinate.to_excel('Coordinate.xlsx')
```

> Timeline Activity Table

Main problem in this step is that starting time for each student is not the same. While someone wakes up before 6AM, the other one wakes up after 7AM. While someone sleep before 10PM, the other one still does homework and sleep after midnight.

Indexing in python is very useful in this step. In case that there is no activity in the early morning time, last activity, which normally is sleeping, is used to represent those period.

Random number is applied to make the dot, which represents student, not overlap. Moreover, this makes feeling of continuous movement for each student at each period of time.

```python
import random
import datetime

data = []
df = pd.read_excel('Activities.xlsx')

for s in df['Student'].unique():
    df_s = df[df['Student'] == s]
    for h in range(0, 24):
        for m in range(0, 60, 15):
            time_index = [i for i, j in enumerate(df[df['Student'] == s]['Time']) if j <= datetime.time(h, m)]
            time_index = -1 if not time_index else max(time_index)
            data.append([s,                                                    # Student
                         datetime.time(h, m),                                  # Time
                         df_s.iloc[time_index, df.columns.get_loc('Label')],   # Activity
                         df_s.iloc[time_index, df.columns.get_loc('Pain')],    # Pain
                         df_s.iloc[time_index, df.columns.get_loc('Gain')],    # Gain
                         df_coordinate.loc[df_s.iloc[time_index, df.columns.get_loc('Label')], 'X'] + random.randrange(-220, 220, 1)/1000, # X
                         df_coordinate.loc[df_s.iloc[time_index, df.columns.get_loc('Label')], 'Y'] + random.randrange(-220, 220, 1)/1000  # Y
                        ])
            
df_timeline = pd.DataFrame(data, columns = ['Student', 'Time', 'Activity', 'Pain', 'Gain', 'X', 'Y'])

df_timeline.to_excel('Timeline.xlsx')
```

> Top Activity Tables

At the same period of time, all activities have been counted and ranked. Only the top of that particular time will be shown on pain-gain tables on the left handside.

```python
df_count = df_timeline[['Time', 'Activity', 'Student']].groupby(['Time', 'Activity']).count().reset_index()
df_count['Percent'] = df_count['Student']/len(df['Student'].unique())*100

df_top = df_count[['Time', 'Student']].groupby(['Time']).max().reset_index()
df_top = pd.merge(df_top, df_count, on = ['Time', 'Student'])
df_top = df_top.drop(['Student', 'Percent'], axis = 1).drop_duplicates('Time')
df_top_pg = pd.merge(df_top, df_timeline[['Time', 'Activity', 'Pain', 'Gain']], on = ['Time', 'Activity'])

df_top.to_excel('TopActivity.xlsx')
df_top_pg.to_excel('TopPainGain.xlsx')
```

### Visualization

All above tables are imported to Power BI for visualization purpose. Data schema can be summarized as picture below.

There are 2 primary keys, including Activity in `ActivityCoordinate` table and Time in `ActivityTop` table. Connection is `one-to-many` with `both` cross filter direction.

With drag and drop feature in Power BI, the visualization as in animation above can be created. However, lebel on the circle will show only when there are some student done that activity at that time. So, `OfflineCoordinate` is used as backgroup coordinate which will not update any time.

The remaining is cosmetic work which depends on editor's taste.

![](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2003/Picture%203-2%20Schema.jpg)
