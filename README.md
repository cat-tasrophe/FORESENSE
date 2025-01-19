# FORESENSE
Integration of SW520D tilt sensor, MQ-2 gas sensor, and active buzzer as an alarm system.

    from machine import Pin, ADC
    import time
    
    gas_sensor = ADC(Pin(26)) 
    tilt_sensor = Pin(15, Pin.IN)
    buzzer = Pin(16, Pin.OUT)
    switch = Pin(14, Pin.IN, Pin.PULL_UP) 
    
    threshold = 14500 
    
    last_trigger_time = 0
    debounce_delay = 5 

    while True:

    if switch.value() == 0:  # Switch pressed (active low)
        print("System Activated!")
   
        sensor_value = gas_sensor.read_u16()
        print("Gas Sensor Value:", sensor_value)
      
        if sensor_value > threshold:  
            print("Fire-related gas detected! Triggering alarm...")
            buzzer.value(1)  
        else:
            print("No fire-related gas detected.")
            buzzer.value(0)  
       
        if tilt_sensor.value() == 1: 
            print("Tilt detected! Triggering alarm...")
            buzzer.value(1) 
        else:
            print("No tilt detected.")
            buzzer.value(0) 
        
    else:
        print("System Deactivated.")
        buzzer.value(0)  
    
    time.sleep(1)
