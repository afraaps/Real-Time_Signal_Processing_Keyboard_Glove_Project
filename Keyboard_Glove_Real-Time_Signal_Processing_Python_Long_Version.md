###### Note: The WIFI implemetation is not mentioned in this repository, you can use which ever wifi protocol you prefer for this and then apply the following real time signal processing methods to your data. It would be best to set up the wifi protocol after the _Import libaries._ section.
***
# Import libaries.
        from scipy import signal
        from math import cos, pi, sin
        from re import M
        import pyaudio, struct
        import tkinter as Tk  
        
        import wave
        import numpy as np
        from matplotlib import pyplot as plt
***
### _Set up WIFI or BLE section of your code here to take reading from the microcontroller and sensors._
***

# Set up place to store data from hardware with WIFI protocol
##### We set up five string type lists to store the date here since 5 strain sensors for 5 fingers are used, and the wifi protocol I used sends data in string type. Change this to what ever works best for your WIFI protocol.

        global line
        global line2
        global line3
        global line4
        global line5
        
        line = ['-1']
        line2 = ['-1']
        line3 = ['-1']
        line4 = ['-1']
        line5 = ['-1']
***
# Keyboard Sound Setup 
        DBscale = False
        #DBscale = True
        
        BLOCKLEN    = 128       # Number of frames per block
        WIDTH       = 2         # Bytes per sample
        CHANNELS    = 1         # Mono
        RATE        = 2000      # Frames per second
        MAXVALUE    = 2**15-1   # Maximum allowed output signal value (because WIDTH = 2)

# To create the keyboard sound effects use a second order infinite impulse response (IIR) filter and clipping of the data was implemented.
## Setting up second order IIR filter
### Parameters
#### I used piano frequecy notes, but you can change these to be at the desired frequency you prefer. [Link](https://en.wikipedia.org/wiki/Piano_key_frequencies) [Link](https://homes.luddy.indiana.edu/donbyrd/Teach/MusicalPitchesTable.htm)

### Note: For the WIFI protocol I used this decay time was suffience, make sure to adjust this based on your data transmision rate from the microcontroller to your PC/python-code.
        Ta = 0.020       # Decay time (seconds) 

### Five sets of keys.
## Set One:
        f1 = 7902.133    # Frequency (Hz)
        f2 = 7458.620    # Frequency (Hz)
        f3 = 7040.000    # Frequency (Hz)
        f4 = 6644.875    # Frequency (Hz)
        f5 = 6271.927    # Frequency (Hz)
 ## Set Two:       
        f1_2 = 5919.911    # Frequency (Hz)
        f2_2 = 5587.652    # Frequency (Hz)
        f3_2 = 5274.041    # Frequency (Hz)
        f4_2 = 4978.032    # Frequency (Hz)
        f5_2 = 4698.636    # Frequency (Hz)
## Set Three:        
        f1_3 = 4434.922    # Frequency (Hz)
        f2_3 = 4186.009    # Frequency (Hz)
        f3_3 = 3951.066    # Frequency (Hz)
        f4_3 = 3729.310    # Frequency (Hz)
        f5_3 = 3520.000    # Frequency (Hz)
## Set Four:        
        f1_4 = 3322.438    # Frequency (Hz)
        f2_4 = 3135.963    # Frequency (Hz)
        f3_4 = 2959.955    # Frequency (Hz)
        f4_4 = 2793.826    # Frequency (Hz)
        f5_4 = 2637.020    # Frequency (Hz)
 ## Set Five:      
        f1_5 = 2489.016    # Frequency (Hz)
        f2_5 = 2349.318    # Frequency (Hz)
        f3_5 = 2217.461    # Frequency (Hz)
        f4_5 = 2093.005    # Frequency (Hz)
        f5_5 = 1975.533    # Frequency (Hz)

# Pole radius and angle to be used to get filter coefficients.
##### We use the desired frequency from each key sound obtain the pole radius and angle (pole and zero information) to obtain the filter coefficients that will change the filter and therefore the data/impulse, based on the desired frequency.

###### Angle: (in radians) around the unit circle (from 0 to 2π) corresponds to angular frequency
###### Radius: (from 0 to ∞) corresponds to the strength of the pole or zero at that frequency

##### Poles (a) and zeros (b) relate to the frequency response of a system.
###### [Link](https://dspfirst.gatech.edu/chapters/08feedbac/demos/recur/index.html) [Link](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html)

        r = 0.1**(1.0/(Ta*RATE))        
        om1 = 2.0 * pi * float(f1)/RATE
        om2 = 2.0 * pi * float(f2)/RATE
        om3 = 2.0 * pi * float(f3)/RATE
        om4 = 2.0 * pi * float(f4)/RATE
        om5 = 2.0 * pi * float(f5)/RATE
        
        om1_2 = 2.0 * pi * float(f1_2)/RATE
        om2_2 = 2.0 * pi * float(f2_2)/RATE
        om3_2 = 2.0 * pi * float(f3_2)/RATE
        om4_2 = 2.0 * pi * float(f4_2)/RATE
        om5_2 = 2.0 * pi * float(f5_2)/RATE
        
        om1_3 = 2.0 * pi * float(f1_3)/RATE
        om2_3 = 2.0 * pi * float(f2_3)/RATE
        om3_3 = 2.0 * pi * float(f3_3)/RATE
        om4_3 = 2.0 * pi * float(f4_3)/RATE
        om5_3 = 2.0 * pi * float(f5_3)/RATE
        
        om1_4 = 2.0 * pi * float(f1_4)/RATE
        om2_4 = 2.0 * pi * float(f2_4)/RATE
        om3_4 = 2.0 * pi * float(f3_4)/RATE
        om4_4 = 2.0 * pi * float(f4_4)/RATE
        om5_4 = 2.0 * pi * float(f5_4)/RATE
        
        om1_5 = 2.0 * pi * float(f1_5)/RATE
        om2_5 = 2.0 * pi * float(f2_5)/RATE
        om3_5 = 2.0 * pi * float(f3_5)/RATE
        om4_5 = 2.0 * pi * float(f4_5)/RATE
        om5_5 = 2.0 * pi * float(f5_5)/RATE

# Calculating the filter coefficients for second-order IIR filter. [Link](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.lfilter.html) 
###### You should simulate transfer functions in Matlab to test the poles and zeros and make any adjustments. 

        a = [1, -2*r*cos(om1), r**2]
        b = [r*sin(om1)]
        
        a2 = [1, -2*r*cos(om2), r**2]
        b2 = [r*sin(om2)]
        
        a3 = [1, -2*r*cos(om3), r**2]
        b3 = [r*sin(om3)]
        
        a4 = [1, -2*r*cos(om4), r**2]
        b4 = [r*sin(om4)]
        
        a5 = [1, -2*r*cos(om5), r**2]
        b5 = [r*sin(om5)]
        
        
        
        a_2 = [1, -2*r*cos(om1_2), r**2]
        b_2 = [r*sin(om1_2)]
        
        a2_2 = [1, -2*r*cos(om2_2), r**2]
        b2_2 = [r*sin(om2_2)]
        
        a3_2 = [1, -2*r*cos(om3_2), r**2]
        b3_2 = [r*sin(om3_2)]
        
        a4_2 = [1, -2*r*cos(om4_2), r**2]
        b4_2 = [r*sin(om4_2)]
        
        a5_2 = [1, -2*r*cos(om5_2), r**2]
        b5_2 = [r*sin(om5_2)]
        
        
        
        a_3 = [1, -2*r*cos(om1_3), r**2]
        b_3 = [r*sin(om1_3)]
        
        a2_3 = [1, -2*r*cos(om2_3), r**2]
        b2_3 = [r*sin(om2_3)]
        
        a3_3 = [1, -2*r*cos(om3_3), r**2]
        b3_3 = [r*sin(om3_3)]
        
        a4_3 = [1, -2*r*cos(om4_3), r**2]
        b4_3 = [r*sin(om4_3)]
        
        a5_3 = [1, -2*r*cos(om5_3), r**2]
        b5_3 = [r*sin(om5_3)]
        
        
        a_4 = [1, -2*r*cos(om1_4), r**2]
        b_4 = [r*sin(om1_4)]
        
        a2_4 = [1, -2*r*cos(om2_4), r**2]
        b2_4 = [r*sin(om2_4)]
        
        a3_4 = [1, -2*r*cos(om3_4), r**2]
        b3_4 = [r*sin(om3_4)]
        
        a4_4 = [1, -2*r*cos(om4_4), r**2]
        b4_4 = [r*sin(om4_4)]
        
        a5_4 = [1, -2*r*cos(om5_4), r**2]
        b5_4 = [r*sin(om5_4)]
        
        
        a_5 = [1, -2*r*cos(om1_5), r**2]
        b_5 = [r*sin(om1_5)]
        
        a2_5 = [1, -2*r*cos(om2_5), r**2]
        b2_5 = [r*sin(om2_5)]
        
        a3_5 = [1, -2*r*cos(om3_5), r**2]
        b3_5 = [r*sin(om3_5)]
        
        a4_5 = [1, -2*r*cos(om4_5), r**2]
        b4_5 = [r*sin(om4_5)]
        
        a5_5 = [1, -2*r*cos(om5_5), r**2]
        b5_5 = [r*sin(om5_5)]

# Filter order, store sound being created, and general set up.
        ORDER = 2  
        states = np.zeros(ORDER)
        
        x = np.zeros(BLOCKLEN)
        x2 = np.zeros(BLOCKLEN)
        x3 = np.zeros(BLOCKLEN)
        x4 = np.zeros(BLOCKLEN)
        x5 = np.zeros(BLOCKLEN)

# Open the audio output stream
###### specify low _frames_per_buffer_ to reduce latency
        p = pyaudio.PyAudio()
        PA_FORMAT = pyaudio.paInt16
        stream = p.open(
                format      = PA_FORMAT,
                channels    = CHANNELS,
                rate        = RATE,
                input       = False,
                output      = True,
                frames_per_buffer = 128*2*2)
***
### WRITE AND READ FILE 
        Fs = 16000
        f0 = 400000
        
        MIN = -(2**15-1)
        MAX = (2**15-1)

       
***
# GUI
        CONTINUE = True
        KEYPRESS = False
        
### Function to quit the code quickly.
        def fun_quit():
          global CONTINUE
          print('Good bye')
          CONTINUE = False
        
### Function is used to change the octaves of the keyboard 'keys' set.
        def ch(event):                 # call back function
            global change
            if event.char == 'n':      # increases frequecy by a full octave
              change = change + 1      # increase frequency
            if event.char == 'b':
              change = change - 1      # decrease frequency
        
            print('change')
            print(change)
        
        change = 0

# Define TK root
        root = Tk.Tk()
        root.bind("<Key>", ch) # binds key to GUI
        
        B_quit = Tk.Button(root, text = 'Quit', command = fun_quit)
        B_quit.pack()

***
###### _dpk_ and _pk_ are max and min values for the sensor data, respectively. For more complex hand gestures, set up more ranges of motions/data for each finger/sensor and correlate the filtering effects to them. As this repository is focused on the signal processing aspects the min and max is all that will be used here. 
        pk = str('0')
        dpk = str('4095')
***             
        one = np.chararray((2, 1))
        one[:] = '-1'
        
        two = np.chararray((2, 1))
        two[:] = '-1'
        
        three = np.chararray((2, 1))
        three[:] = '-1'
        
        four = np.chararray((2, 1))
        four[:] = '-1'
        
        five = np.chararray((2, 1))
        five[:] = '-1'
        
        
        global i
        i = 0
        
        k1 = 0 
        k2 = 0 
        k3 = 0 
        k4 = 0 
        k5 = 0  
        
 # Where the filtering and audio generation takes place. 
        while CONTINUE:
            root.update()
            i+=1
        
            if i > 1:
                i = 0
                
            if change > 0:
                change = 0
            
            if change < 0:
                change = 0
        
            if line == dpk:
                x[k1] = 10000.0
                print("Pinky")
                x = x
###### Changes between keysets/octaves.
            if change == 0:
                a = a
                b = b
        
            if change == 1:
                a_2 = a_2
                b_2 = a_2
        
            if change == 2:
                a_3 = a_3
                b_3 = b_3
        
            if change == 3:
                a_4 = a_4
                b_4 = b_4
        
            if change == 4:
                a_5 = a_5
                b_5 = b_5
***             
            if line2 == dpk:
                x2[0] = 10000.0
                print("Ring")
                x += x2         # To play account for mulitple keys being played simultaniously everything is stored in x and then the filter is applied to this.

            if change == 0:
                a = a2
                b = b2
        
            if change == 1:
                a_2 = a2_2
                b_2 = b2_2
        
            if change == 2:
                a_3 = a2_3
                b_3 = b2_3
            
            if change == 3:
                a_4 = a2_4
                b_4 = b2_4
        
            if change == 4:
                a_5 = a2_5
                b_5 = b2_5

***
            if line3 == dpk:
                x3[0] = 10000.0
                print("Middle")
                x += x3        # To play account for mulitple keys being played simultaniously everything is stored in x and then the filter is applied to this.
        
            if change == 0:
                a = a3
                b = b3
        
            if change == 1:
                a_2 = a3_2
                b_2 = b3_2
        
            if change == 2:
                a_3 = a3_3
                b_3 = b3_3
        
            if change == 3:
                a_4 = a3_4
                b_4 = b3_4
            
            if change == 4:
                a_5 = a3_5
                b_5 = b3_5
***        
            if line4 == dpk:
                x4[0] = 10000.0
                print("Index")
                x += x4        # To play account for mulitple keys being played simultaniously everything is stored in x and then the filter is applied to this.
                
            if change == 0:
                a = a4
                b = b4
        
            if change == 1:
                a_2 = a4_2
                b_2 = b4_2
        
            if change == 2:
                a_3 = a4_3
                b_3 = b4_3
        
            if change == 3:
                a_4 = a4_4
                b_4 = b4_4
        
            if change == 4:
                a_5 = a4_5
                b_5 = b4_5
***        
            if line5 != pk:
                x5[0] = 10000.0
                print("Thump")
                x += x5        # To play account for mulitple keys being played simultaniously everything is stored in x and then the filter is applied to this.
        
            if change == 0:
                a = a5
                b = b5
            
            if change == 1:
                a_2 = a5_2
                b_2 = b5_2
        
            if change == 2:
                a_3 = a5_3
                b_3 = b5_3
        
            if change == 3:
                a_4 = a5_4
                b_4 = b5_4
        
            if change == 4:
                a_5 = a5_5
                b_5 = b5_5
*** 
### Various sets of filter coefficients choosen based on user input.
###### Apply filter to signal here. Second order IIR filter.
            [y, states] = signal.lfilter(b, a, x, zi = states)
###### Filters data in a manner similar to a bandpass filter.
            y = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)        
           
            [y_2, states] = signal.lfilter(b_2, a_2, x, zi = states)
            y_2 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     
        
            [y_3, states] = signal.lfilter(b_3, a_3, x, zi = states)
            y_3 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     
        
            [y_4, states] = signal.lfilter(b_4, a_4, x, zi = states)
            y_4 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     
            
            [y_5, states] = signal.lfilter(b_5, a_5, x, zi = states)
            y_5 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)   
***
###### Resets the values from sensor input here to disregard previous sensor inputs.
            x[0] = 0.0
            x2[0] = 0.0
            x3[0] = 0.0
            x4[0] = 0.0
            x5[0] = 0.0
            
#### Steams out audio.
            if change == 0:
                binary_data = struct.pack('h' * BLOCKLEN, *y)
                stream.write(binary_data, BLOCKLEN)   
            
            if change == 1:
                binary_data = struct.pack('h' * BLOCKLEN, *y_2)
                stream.write(binary_data, BLOCKLEN)  
        
            if change == 2:
                binary_data = struct.pack('h' * BLOCKLEN, *y_3)
                stream.write(binary_data, BLOCKLEN)  
        
            if change == 3:
                binary_data = struct.pack('h' * BLOCKLEN, *y_4)
                stream.write(binary_data, BLOCKLEN)  
        
            if change == 4:
                binary_data = struct.pack('h' * BLOCKLEN, *y_5)
                stream.write(binary_data, BLOCKLEN)
        
###### Update sensor input values in this section (depending on your WIFI protocol) at the end of the while loop.   
##### Updates sensor input values based on my WIFI protocol.
            if line:
                #print(" line")
                #print( line)
                one[i] =  line
        
            if line2:
                #print("line2")
                #print(line2)
                two[i] =  line2
        
            if line3:
                #print("line3")
                #print(line3)
                three[i] = line3
        
            if line4:
                #print("line4")
                #print(line4)
                four[i] = line4
        
            if line5:
                #print("line5")
                #print(line5)
                five[i] =  line5
***  
### Ends audio output when you quit with GUI (section is written after/outside the while loop).
        stream.stop_stream()
        stream.close()
        p.terminate()
