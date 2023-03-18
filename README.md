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


![Capture](https://user-images.githubusercontent.com/24644072/222937008-0d351cee-ab86-4cf1-92ab-c559d5354675.PNG)





![Capture1](https://user-images.githubusercontent.com/24644072/222937011-aa1d0e03-02f6-45ca-94ca-9bf7fa2f3d9f.PNG)

