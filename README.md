# DataAirbnb
doing an analysis for the dataset about Florida
## For the following analysis, I have downloaded the following data from: https://insideairbnb.com/get-the-data/
``` diff
import pandas as pd
DF = pd.read_csv("calendar.csv")
```
# :white_check_mark: We need to know the number of available and unavailable rooms
``` diff
DF.available.value_counts()
```

* T stands for avilable, F stand for unavailable
