# Climate-Analysis-Sqlalchemy

%matplotlib inline
from matplotlib import style
style.use('fivethirtyeight')
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import datetime as dt
from pprint import pprint
Reflect Tables into SQLAlchemy ORM
# Python SQL toolkit and Object Relational Mapper
import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func
# create engine to hawaii.sqlite
engine = create_engine("sqlite:///Resources/hawaii.sqlite", echo=False)
# reflect an existing database into a new model
Base = automap_base()

# reflect the tables
Base.prepare(autoload_with=engine)
# View all of the classes that automap found
Base.classes.keys()
['measurement', 'station']
# Save references to each table
measurement = Base.classes.measurement
station = Base.classes.station
# Create our session (link) from Python to the DB
session = Session(engine)
session.execute('SELECT * FROM measurement LIMIT 5').fetchall()
[(1, 'USC00519397', '2010-01-01', 0.08, 65.0),
 (2, 'USC00519397', '2010-01-02', 0.0, 63.0),
 (3, 'USC00519397', '2010-01-03', 0.0, 74.0),
 (4, 'USC00519397', '2010-01-04', 0.0, 76.0),
 (5, 'USC00519397', '2010-01-06', None, 73.0)]
Exploratory Precipitation Analysis
# Find the most recent date in the data set.
recent_date = session.query(measurement.date).order_by(measurement.date.desc()).first()[0]
recent_date = dt.datetime.strptime(recent_date, "%Y-%m-%d")                                                  
# Design a query to retrieve the last 12 months of precipitation data and plot the results. 
# Starting from the most recent data point in the database. 
#recent_date -
# Calculate the date one year from the last date in data set.
last_year =  recent_date - dt.timedelta(days=365)

# Perform a query to retrieve the data and precipitation scores
precip_scores = (
                session.query(measurement.date, measurement.prcp)
                .filter(measurement.date > last_year)
                .order_by(measurement.date)
                .all()
                ) 

# Save the query results as a Pandas DataFrame and set the index to the date column
precip_scores_df = pd.DataFrame(precip_scores, columns=["Date","Precipitation"])
precip_scores_df["Date"] = pd.to_datetime(precip_scores_df["Date"])


# Sort the dataframe by date
precip_scores_df = precip_scores_df.sort_values("Date").dropna().set_index("Date")

# Use Pandas Plotting with Matplotlib to plot the data
precip_scores_df.plot(color='blue',figsize=(9, 8))
plt.title("Precipitation Data for Last 12 Months")
plt.xlabel("Dates")
plt.ylabel("Precipitation (mmm)" )

#precip_scores_df
Text(0, 0.5, 'Precipitation (mmm)')
