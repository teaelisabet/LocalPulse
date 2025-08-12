# Import libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from io import StringIO

# 1. Data Preparation 
data = """
Date,Business_ID,Search_Volume_Term,Average_Review_Score,Review_Sentiment_Score,Foot_Traffic,Hashtag_Mentions,Local_Event,Weather,Business_Type,Revenue
2024-01-01,101,Coffee Shop Near Me,4.5,0.85,120,55,No,Sunny,Cafe,1500
2024-01-01,102,Taco Tuesday Deal,4.1,0.65,85,32,No,Sunny,Restaurant,1100
2024-01-01,103,Yoga Class,4.8,0.92,45,15,No,Sunny,Fitness,750
2024-01-02,101,Coffee Shop Near Me,4.6,0.90,135,60,No,Cloudy,Cafe,1650
2024-01-02,102,Taco Tuesday Deal,4.2,0.70,90,35,No,Cloudy,Restaurant,1200
2024-01-02,103,Yoga Class,4.9,0.95,50,18,No,Cloudy,Fitness,800
2024-01-03,101,Coffee Shop Near Me,4.4,0.78,110,50,No,Rainy,Cafe,1400
2024-01-03,102,Taco Tuesday Deal,4.0,0.60,75,28,No,Rainy,Restaurant,1000
2024-01-03,103,Yoga Class,4.7,0.88,40,12,No,Rainy,Fitness,700
2024-01-04,101,Coffee Shop Near Me,4.7,0.91,150,70,Local Event,Sunny,Cafe,1800
2024-01-04,102,Taco Tuesday Deal,4.3,0.75,100,40,Local Event,Sunny,Restaurant,1350
2024-01-04,103,Yoga Class,5.0,0.98,60,25,Local Event,Sunny,Fitness,950
2024-01-05,101,Coffee Shop Near Me,4.6,0.87,140,65,No,Cloudy,Cafe,1700
2024-01-05,102,Taco Tuesday Deal,4.4,0.80,95,38,No,Cloudy,Restaurant,1250
2024-01-05,103,Yoga Class,4.9,0.94,55,20,No,Cloudy,Fitness,850
"""

df = pd.read_csv(StringIO(data))

# 2. Preprocessing
df = pd.get_dummies(df, columns=['Local_Event', 'Weather', 'Business_Type'], drop_first=True)

# Define (X) and (y).
# target = Revenue
# Drop irrelevant columns
features = df.drop(columns=['Date', 'Business_ID', 'Search_Volume_Term', 'Revenue'])
target = df['Revenue']

# 3. Model Training
# training vs. testing sets, 80%, 20%
X_train, X_test, y_train, y_test = train_test_split(features, target, test_size=0.2, random_state=42)

# Linear Regression
model = LinearRegression()

# train model 
model.fit(X_train, y_train)

# 4. Prediction
predictions = model.predict(X_test)

# Print the model's coefficients
print("Model Coefficients:")
for feature, coef in zip(features.columns, model.coef_):
    print(f"  {feature}: {coef:.2f}")

sample_input = X_test.iloc[0].values.reshape(1, -1)
sample_prediction = model.predict(sample_input)
actual_value = y_test.iloc[0]

print("\nSample Prediction:")
print(f"  Features used for prediction: {sample_input}")
print(f"  Predicted Revenue: ${sample_prediction[0]:.2f}")
print(f"  Actual Revenue: ${actual_value:.2f}")
