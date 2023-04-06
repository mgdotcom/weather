import pandas as pd
import numpy as np
import requests
from datetime import datetime, timedelta
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

# Get historical weather data from NOAA
start_date = datetime.today() - timedelta(days=365*5)
end_date = datetime.today() - timedelta(days=1)
url = f"https://www.ncei.noaa.gov/access/services/data/v1?dataset=daily-summaries&startDate={start_date.strftime('%Y-%m-%d')}&endDate={end_date.strftime('%Y-%m-%d')}&stations=ZAF00000655&dataTypes=TMAX,TMIN&format=csv"
response = requests.get(url)
data = pd.read_csv(response.content)

# Preprocess the data
data["Date"] = pd.to_datetime(data["DATE"], format="%Y-%m-%d")
data["Max Temp"] = data["TMAX"] / 10.0
data["Min Temp"] = data["TMIN"] / 10.0
data = data[["Date", "Max Temp", "Min Temp"]]
data.set_index("Date", inplace=True)
data = data.resample("D").mean().fillna(method="ffill").reset_index()
data["Month"] = data["Date"].dt.month
data["Day"] = data["Date"].dt.day
data = data.drop(["Date"], axis=1)
X = data.drop(["Max Temp"], axis=1) # Input features
y = data["Max Temp"] # Output variable
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Train the model
model = RandomForestRegressor(n_estimators=100)
model.fit(X_train, y_train)

# Make predictions for the next 90 days
future_dates = pd.date_range(end_date + timedelta(days=1), end_date + timedelta(days=90), freq="D")
future_data = pd.DataFrame({
    "Month": future_dates.month,
    "Day": future_dates.day
})
predictions = model.predict(future
