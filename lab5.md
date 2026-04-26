# Non-Inverting Amplifier Design Calculations

def design_non_inverting_amp(target_gain, vin_pp, vcc):
    """
    Calculates Resistor values and checks for clipping.
    """
    # 1. Assume a standard value for R1 (typically 1k to 10k ohms)
    r1 = 1000  # 1 kOhm
    
    # 2. Calculate Rf using Gain formula: Av = 1 + (Rf / R1)
    # Rf = (Av - 1) * R1
    rf = (target_gain - 1) * r1
    
    # 3. Calculate expected Vout peak
    vin_p = vin_pp / 2
    vout_p_calc = target_gain * vin_p
    
    # 4. Check for saturation (Op-amp usually saturates ~1-2V below Vcc)
    v_sat = vcc - 1.5 
    is_saturated = vout_p_calc > v_sat
    
    print(f"--- Design Report ---")
    print(f"Target Gain: {target_gain} V/V")
    print(f"Selected R1: {r1} Ohms")
    print(f"Calculated Rf: {rf} Ohms")
    print(f"Input Peak: {vin_p} V")
    print(f"Calculated Output Peak: {vout_p_calc} V")
    
    if is_saturated:
        print(f"WARNING: Output will saturate! Max swing is approx ±{v_sat}V.")
        print(f"To avoid clipping, max Vin_pp should be: {(v_sat / target_gain) * 2:.2f} Vpp")
    else:
        print("Status: Circuit will operate in linear region.")

# Inputs from the user's problem statement
design_non_inverting_amp(target_gain=5, vin_pp=12, vcc=15)
