pip install requests matplotlib seaborn pandas #install require libraries

import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Step 1: API Key and Endpoint
API_KEY = 'your_api_key_here'  # Replace with your OpenWeatherMap API Key
CITY = 'Chennai'
URL = f"http://api.openweathermap.org/data/2.5/forecast?q={CITY}&appid={API_KEY}&units=metric"

# Step 2: Fetch Data
response = requests.get(URL)
data = response.json()

# Step 3: Extract Relevant Information
forecast_list = data['list']
weather_data = {
    "DateTime": [],
    "Temperature": [],
    "Humidity": [],
    "Weather": []
}

for item in forecast_list:
    weather_data["DateTime"].append(item["dt_txt"])
    weather_data["Temperature"].append(item["main"]["temp"])
    weather_data["Humidity"].append(item["main"]["humidity"])
    weather_data["Weather"].append(item["weather"][0]["main"])

df = pd.DataFrame(weather_data)

# Step 4: Visualization using Matplotlib and Seaborn
plt.figure(figsize=(14, 6))
sns.lineplot(x="DateTime", y="Temperature", data=df, marker='o')
plt.xticks(rotation=45)
plt.title(f"Temperature Forecast for {CITY}")
plt.xlabel("Date & Time")
plt.ylabel("Temperature (°C)")
plt.tight_layout()
plt.show()

# Humidity Plot
plt.figure(figsize=(14, 6))
sns.lineplot(x="DateTime", y="Humidity", data=df, marker='o', color='orange')
plt.xticks(rotation=45)
plt.title(f"Humidity Forecast for {CITY}")
plt.xlabel("Date & Time")
plt.ylabel("Humidity (%)")
plt.tight_layout()
plt.show()
