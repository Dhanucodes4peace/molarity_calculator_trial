import streamlit as st
import pandas as pd

# Function to calculate the required mass for a given molarity and volume
def calculate_required_mass(molar_mass, molarity, volume, concentration_unit, volume_unit):
    if concentration_unit == "Millimolar":
        molarity /= 1000  # Convert to Molar
    if volume_unit == "mL":
        volume /= 1000  # Convert to Liters
    elif volume_unit == "μL":
        volume /= 1_000_000  # Convert to Liters
    
    required_mass = molarity * volume * molar_mass
    return required_mass

# Streamlit UI
st.title("Molarity Calculator")

# Form input fields
molar_mass = st.number_input("Molar Mass (g/mol)", min_value=0.0)
molarity = st.number_input("Desired Concentration (M)", min_value=0.0)
concentration_unit = st.selectbox("Concentration Unit", ["Molar", "Millimolar"])
volume = st.number_input("Desired Volume", min_value=0.0)
volume_unit = st.selectbox("Volume Unit", ["mL", "L", "μL"])

# Add button to store inputs
if st.button("Add to Inputs"):
    if molar_mass == 0.0 or molarity == 0.0 or volume == 0.0:
        st.error("Please fill out all fields correctly.")
    else:
        if 'inputs' not in st.session_state:
            st.session_state.inputs = []
        st.session_state.inputs.append({
            'molar_mass': molar_mass,
            'molarity': molarity,
            'volume': volume,
            'concentration_unit': concentration_unit,
            'volume_unit': volume_unit
        })
        st.success("Input added. You can add more or click Calculate to see results.")

# Calculate button
if st.button("Calculate"):
    if 'inputs' in st.session_state and len(st.session_state.inputs) > 0:
        results = []
        for input_data in st.session_state.inputs:
            required_mass = calculate_required_mass(
                input_data['molar_mass'],
                input_data['molarity'],
                input_data['volume'],
                input_data['concentration_unit'],
                input_data['volume_unit']
            )
            results.append({
                'Molar Mass (g/mol)': input_data['molar_mass'],
                'Concentration': input_data['molarity'],
                'Unit': input_data['concentration_unit'],
                'Volume': input_data['volume'],
                'Volume Unit': input_data['volume_unit'],
                'Required Mass (g)': required_mass
            })
        
        df_results = pd.DataFrame(results)
        st.table(df_results)
        
        # Generate CSV download link
        csv = df_results.to_csv(index=False)
        st.download_button("Download Results", data=csv, file_name="molarity_calculator_results.csv")
    else:
        st.error("Please add some inputs first.")
