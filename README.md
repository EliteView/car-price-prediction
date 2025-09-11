import streamlit as st
import pandas as pd
import joblib

# Load the trained model
model = joblib.load('Car_Price_Predictor')

st.title("Car Price Prediction App")

st.write("Enter the details of the car:")

present_price = st.number_input("Present Price (in lakhs)", min_value=0.0, step=0.01)
kms_driven = st.number_input("Kms Driven", min_value=0, step=1)
fuel_type = st.selectbox("Fuel Type", ["Petrol", "Diesel", "CNG"])
seller_type = st.selectbox("Seller Type", ["Dealer", "Individual"])
transmission = st.selectbox("Transmission", ["Manual", "Automatic"])
owner = st.selectbox("Owner", [0, 1, 2, 3])

# Map categorical values to numbers as per your model
fuel_type_map = {"Petrol": 0, "Diesel": 1, "CNG": 2}
seller_type_map = {"Dealer": 0, "Individual": 1}
transmission_map = {"Manual": 0, "Automatic": 1}

if st.button("Predict"):
    input_df = pd.DataFrame({
        "Present_Price": [present_price],
        "Kms_Driven": [kms_driven],
        "Fuel_Type": [fuel_type_map[fuel_type]],
        "Seller_Type": [seller_type_map[seller_type]],
        "Transmission": [transmission_map[transmission]],
        "Owner": [owner]
    })
    prediction = model.predict(input_df)[0]
    st.success(f"Estimated Selling Price: ₹ {prediction:.2f} lakhs")
