## Sections of Real Time Signal Processing Code in Python
***
### Playing Keyboard Sound
#### To create the keyboard sound effects a second order infinite impulse response filter and clipping of the data was implemented. 

#### Setting up second order IIR filter
    DBscale = False
    
    BLOCKLEN    = 64*2 # Number of frames per block
    WIDTH       = 2         # Bytes per sample
    CHANNELS    = 1         # Mono
    RATE        = 2000     # Frames per second
    MAXVALUE    = 2**15-1  # Maximum allowed output signal value (because WIDTH = 2)
    
    # Parameters
    Ta = 0.020        # Decay time (seconds)
    f1 = 7902.133    # Frequency (Hz) 
    f2 = 7458.620    # Frequency (Hz)
    f3 = 7040.000    # Frequency (Hz)
    f4 = 6644.875    # Frequency (Hz)
    f5 = 6271.927    # Frequency (Hz)

  ***
#### Pole radius and angle for filter coefficients 
    r = 0.1**(1.0/(Ta*RATE))             # 0.1 for 10 percent amplitude
    om1 = 2.0 * pi * float(f1)/RATE
    om2 = 2.0 * pi * float(f2)/RATE
    om3 = 2.0 * pi * float(f3)/RATE
    om4 = 2.0 * pi * float(f4)/RATE
    om5 = 2.0 * pi * float(f5)/RATE

### Getting the filter coefficients
#### Filter coefficients (second-order IIR)
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

##### With the desired frequency from each key sound obtain the pole radius and angle (pole and zero information) to obtain the filter coefficients that will change the filter and therefore the data, based on the desired frequency
##### Poles and zeros relate to the frequency response.
###### Angle: (in radians) around the unit circle (from 0 to 2π) corresponds to angular frequency
###### Radius: (from 0 to ∞) corresponds to the strength of the pole or zero at that frequency


##### Set up filter order

    ORDER = 2                  # Filter order 
    states = np.zeros(ORDER)


##### Store sound being created, so filter can be applied to it.
    x = np.zeros(BLOCKLEN)
    x2 = np.zeros(BLOCKLEN)
    x3 = np.zeros(BLOCKLEN)
    x4 = np.zeros(BLOCKLEN)
    x5 = np.zeros(BLOCKLEN)
    
    # Open the audio output stream
    p = pyaudio.PyAudio()
    
##### Opening audio output stream so the audio can be created, stored and/or played.
    PA_FORMAT = pyaudio.paInt16
    
    stream = p.open(
            format      = PA_FORMAT,
            channels    = CHANNELS,
            rate        = RATE,
            input       = False,
            output      = True,
            frames_per_buffer = 128*2*2)


#### Applying Second order IIR filter.

    MIN = -(2**15-1)
    MAX = (2**15-1)

###### Various sets of filter coefficients choosen based on user input.
    [y, states] = signal.lfilter(b, a, x, zi = states)
    y = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)       # Clipping
   
    [y_2, states] = signal.lfilter(b_2, a_2, x, zi = states)
    y_2 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     # Clipping

    [y_3, states] = signal.lfilter(b_3, a_3, x, zi = states)
    y_3 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     # Clipping

    [y_4, states] = signal.lfilter(b_4, a_4, x, zi = states)
    y_4 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     # Clipping

    [y_5, states] = signal.lfilter(b_5, a_5, x, zi = states)
    y_5 = np.clip(y.astype(int), -MAXVALUE, MAXVALUE)     # Clipping
