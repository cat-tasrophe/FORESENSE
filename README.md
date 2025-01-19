# FORESENSE
Integration of SW520D tilt sensor, MQ-2 gas sensor, and active buzzer as an alarm system.

    from machine import Pin, ADC
    import time
    
    gas_sensor = ADC(Pin(26))    #The first setup is the gas sensor, using an analog output for more accurate readings. It is connected to pin 26 or GPIO 26.
    tilt_sensor = Pin(15, Pin.IN)    #The second setup is the tilt sensor, which uses a digital input to quickly sense vibrations. GPIO 15 is the input pin, while Pin.IN reads its state (HIGH or LOW).
    buzzer = Pin(16, Pin.OUT)    #The third setup configures the buzzer as a digital output. GPIO 16 controls its state to sound an alarm.
    switch = Pin(14, Pin.IN, Pin.PULL_UP)    #The fourth setup is the switch, which controls the alarm system. It uses a digital input with a pull-up resistor. The switch works in an active LOW configuration (pulling the pin LOW when pressed). GPIO 14 is the input pin, with Pin.PULL_UP enabling the pull-up resistor.

    
    threshold = 14500     #The threshold value determines the gas sensor’s trigger point. If the sensor reading exceeds this value, the alarm is activated. The value can be adjusted based on environmental testing.

    #Finally, two variables prevent multiple triggers in a short time:
    last_trigger_time = 0    #last_trigger_time records the last alarm activation,
    debounce_delay = 5    #while debounce_delay sets a minimum 5-second interval between triggers. Both sensors share the same buzzer, so this delay avoids false alarms.
    
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
