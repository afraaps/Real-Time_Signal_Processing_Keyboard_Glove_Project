###### Note: The wifi implemetation is not mentioned in this repository, you can use which ever wifi protocol you prefer for this and then apply the following real time signal processing methods to your data.
***
# Import libaries 
    from scipy import signal
    from math import cos, pi, sin
    from re import M
    import pyaudio, struct
    import tkinter as Tk  
    
    import wave
    import numpy as np
    from matplotlib import pyplot as plt

# Set up place to store data from hardware with wifi protocol
##### We set up five string type lists to store the date here since 5 strain sensors for 5 fingers are used
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
    
# Sound 
    DBscale = False
    #DBscale = True
    
    BLOCKLEN    = 64*2     # Number of frames per block
    WIDTH       = 2        # Bytes per sample
    CHANNELS    = 1        # Mono
    RATE        = 2000     # Frames per second
    MAXVALUE    = 2**15-1  # Maximum allowed output signal value (because WIDTH = 2)


# filter order
    ORDER = 2  
    states = np.zeros(ORDER)
    x = np.zeros(BLOCKLEN)
    
    x2 = np.zeros(BLOCKLEN)
    x3 = np.zeros(BLOCKLEN)
    x4 = np.zeros(BLOCKLEN)
    x5 = np.zeros(BLOCKLEN)

# Open the audio output stream
    p = pyaudio.PyAudio()
    PA_FORMAT = pyaudio.paInt16
    stream = p.open(
            format      = PA_FORMAT,
            channels    = CHANNELS,
            rate        = RATE,
            input       = False,
            output      = True,
            frames_per_buffer = 128*2*2)
# specify low frames_per_buffer to reduce latency

##### Set up for GUI to exit program
    CONTINUE = True
    
    def fun_quit():
      global CONTINUE
      print('Good bye')
      CONTINUE = False
      sys.exit()
    
    # Define TK root
    root = Tk.Tk()
    
    B_quit = Tk.Button(root, text = 'Quit', command = fun_quit)
    B_quit.pack()

###### Max and Min for sensor data. For more complex hand gestures, set up more ranges of motions/data for each finger/sensor and correlate the filtering effects to them. As this repository is focused on the signal processing aspects the min and max is all that will be used. 
    pk = str('0')
    dpk = str('4095')
    
    sound = [[b'0']]
    nsound = [[b'4']] 
    
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
***
# Microphone and audio details
    WIDTH       = 2         # Number of bytes per sample
    CHANNELS    = 1         # mono
    RATE        = 16000     # Sampling rate (frames/second)
    DURATION    = 3         # duration of processing (seconds)
    
    N = DURATION * RATE     # N : Number of samples to process

### WRITE AND READ FILE 

    Fs = 16000
    f0 = 400000
    
    MIN = -(2**15-1)
    MAX = (2**15-1)

#### SOUND1 WRITE AND READ 
###### Repeat for other 4 sensors. Change p1, w1, stream1 and Sound1.wav for other for sensors. Ex: p2, w2, stream2 and Sound2.wav for the second sensor. 

    p1 = pyaudio.PyAudio()
    
    w1 = wave.open('Sound1.wav', 'w') #volume 38% to hear and play
    
    w1.setnchannels(1)			# one channels 
    w1.setsampwidth(2)			# two bytes per sample (16 bits per sample)
    w1.setframerate(Fs)			# samples per second
    
    stream1 = p1.open(
        format      = p1.get_format_from_width(WIDTH),
        channels    = CHANNELS,
        rate        = RATE,
        input       = True,
        output      = True)
***
    BLOCKLEN2 = 1024                         # block length of samples
    K = int( DURATION * RATE / BLOCKLEN2 )   # Number of blocks
    
    global save
    save = 0
***
    while CONTINUE:
        root.update()
        #CONTINUE = False
        if line == dpk:
            if one[0] == sound: 
                print("Record For Thumb")
                print("Sound Effect 1: Robot ")
                print('* Start')
    
                save = 1

###### Repeat for line2 to line5
 print(save)

### Filter
    if save == 1:
            for j in range(K):
                input_bytes1_ = stream1.read(BLOCKLEN2)
                #input_bytes1_ = stream1.read(BLOCKLEN2, exception_on_overflow = False)
                signal_block = struct.unpack('h' * BLOCKLEN2, input_bytes1_)  # Convert
        
                f, t, Zxx1 = signal.stft(signal_block, fs = 1024, nperseg = 512 )
                Zxx = np.abs(Zxx1)
                t, y = signal.istft(Zxx, fs = 1024, nperseg = 512 )
            
                y0 = y.astype(int) 
                output_value = np.clip(y0, -MAX, MAX)

                output_bytes = struct.pack('h'* BLOCKLEN2, *y0)    # Convert output value to binary string 
                stream1.write(output_bytes)                        # Write binary string to audio stream
            
                w1.writeframes(output_bytes)   
            print('* Done. 1')
            save = 0
***
        print("...") 
    print('* Done Recording. ')
    save = 0
    record = False
    #CONTINUE = True
    
    stream1.stop_stream()
    stream1.close()
    p1.terminate()   

