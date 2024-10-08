from scipy import signal
from math import cos, pi, sin
from re import M
import pyaudio, struct
import tkinter as Tk  

import wave
import numpy as np
from matplotlib import pyplot as plt

import sys
import socket


################### WiFi ####################################

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
###########################################################

######################## Keyboard Sound Setup ############################
DBscale = False
#DBscale = True

BLOCKLEN    = 64*2      # Number of frames per block
WIDTH       = 2         # Bytes per sample
CHANNELS    = 1         # Mono
RATE        = 2000      # Frames per second
MAXVALUE = 2**15-1      # Maximum allowed output signal value (because WIDTH = 2)

# Parameters
Ta = 0.020       # Decay time (seconds)
f1 = 7902.133    # Frequency (Hz)
f2 = 7458.620    # Frequency (Hz)
f3 = 7040.000    # Frequency (Hz)
f4 = 6644.875    # Frequency (Hz)
f5 = 6271.927    # Frequency (Hz)

f1_2 = 5919.911    # Frequency (Hz)
f2_2 = 5587.652    # Frequency (Hz)
f3_2 = 5274.041    # Frequency (Hz)
f4_2 = 4978.032    # Frequency (Hz)
f5_2 = 4698.636    # Frequency (Hz)

f1_3 = 4434.922    # Frequency (Hz)
f2_3 = 4186.009    # Frequency (Hz)
f3_3 = 3951.066    # Frequency (Hz)
f4_3 = 3729.310    # Frequency (Hz)
f5_3 = 3520.000    # Frequency (Hz)

f1_4 = 3322.438    # Frequency (Hz)
f2_4 = 3135.963    # Frequency (Hz)
f3_4 = 2959.955    # Frequency (Hz)
f4_4 = 2793.826    # Frequency (Hz)
f5_4 = 2637.020    # Frequency (Hz)

f1_5 = 2489.016    # Frequency (Hz)
f2_5 = 2349.318    # Frequency (Hz)
f3_5 = 2217.461    # Frequency (Hz)
f4_5 = 2093.005    # Frequency (Hz)
f5_5 = 1975.533    # Frequency (Hz)

# Pole radius and angle
r = 0.1**(1.0/(Ta*RATE))       # 0.01 for 1 percent amplitude
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

# Filter coefficients (second-order IIR)
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

CONTINUE = False
KEYPRESS = False
record = True
newsound = False
KEY = False

# write function to be true or false for each finger


  

def ch(event): #call back function
    global change

    # increases frequecy by a full octave 
    # if pressed 12 times increases it to a full octave and the frequecy is doubled
    if event.char == 'n': 
      change = change + 1      # increase frequency
   
    if event.char == 'b':
      change = change - 1      # decrease frequency

    print('change')
    print(change)

change = 0

def Key(): #call back function
    global KEY
    global record
    global newsound
    print('KEYBOARD')
    KEY = True
    #record   = False
    #newsound = False
    

def rec(): #call back function
    global KEY
    global record
    global newsound
    print('record')
    record   = True
    newsound = False
    KEY = False

def newsound_fun():
    global KEY
    global record
    global newsound
    print('newsound')
    #newsound = True
    #record   = False
    #KEY = False

def fun_quit():
  global CONTINUE
  global record
  global newsound
  global KEY 
  print('Good bye')
  CONTINUE = False
  record = False
  newsound = False
  KEY = False
  sys.exit()

# Define TK root
root = Tk.Tk()
root.bind("<Key>", ch) # binds key to GUI

B_quit = Tk.Button(root, text = 'Quit', command = fun_quit)
B_quit.pack()

B_rec = Tk.Button(root, text = 'RECORD', command = rec)
B_rec.pack()

B_newsound = Tk.Button(root, text = 'New Sounds', command = newsound_fun)
B_newsound.pack()

B_KEY = Tk.Button(root, text = 'KEYBOARD', command = Key)
B_KEY.pack()

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

k1 = 0 
k2 = 0 
k3 = 0 
k4 = 0 
k5 = 0  


####################### REAL TIME ROBOT FROM MIC ###########################################
WIDTH       = 2         # Number of bytes per sample
CHANNELS    = 1         # mono
RATE        = 16000     # Sampling rate (frames/second)
DURATION    = 3         # duration of processing (seconds)

N = DURATION * RATE     # N : Number of samples to process

###### REAL TIME ROBOT FROM MIC WRITE AND READ FILE #######

Fs = 16000
f0 = 400000

MIN = -(2**15-1)
MAX = (2**15-1)



Fs = 16000
f0 = 400000


########### SOUND1 WRITE AND READ ##################
p1 = pyaudio.PyAudio()


w1 = wave.open('Sound10.wav', 'w') #best to have volume 38% to hear and play simultaniously

w1.setnchannels(1)			# one channels 
w1.setsampwidth(2)			# two bytes per sample (16 bits per sample)
w1.setframerate(Fs)			# samples per second

stream1 = p1.open(
    format      = p1.get_format_from_width(WIDTH),
    channels    = CHANNELS,
    rate        = RATE,
    input       = True,
    output      = True)


########### SOUND2 WRITE AND READ ##################

p2 = pyaudio.PyAudio()

w2 = wave.open('Sound20.wav', 'w') #volume 38% to hear and play

w2.setnchannels(1)			# one channels 
w2.setsampwidth(2)			# two bytes per sample (16 bits per sample)
w2.setframerate(Fs)			# samples per second

stream2 = p2.open(
    format      = p2.get_format_from_width(WIDTH),
    channels    = CHANNELS,
    rate        = RATE,
    input       = True,
    output      = True)

########### SOUND3 WRITE AND READ ##################

p3 = pyaudio.PyAudio()

w3 = wave.open('Sound30.wav', 'w') #volume 38% to hear and play

w3.setnchannels(1)			# one channels 
w3.setsampwidth(2)			# two bytes per sample (16 bits per sample)
w3.setframerate(Fs)			# samples per second

stream3 = p3.open(
    format      = p3.get_format_from_width(WIDTH),
    channels    = CHANNELS,
    rate        = RATE,
    input       = True,
    output      = True)

########### SOUND4 WRITE AND READ ##################

p4 = pyaudio.PyAudio()

w4 = wave.open('Sound40.wav', 'w') #volume 38% to hear and play

w4.setnchannels(1)			# one channels 
w4.setsampwidth(2)			# two bytes per sample (16 bits per sample)
w4.setframerate(Fs)			# samples per second

stream4 = p4.open(
    format      = p4.get_format_from_width(WIDTH),
    channels    = CHANNELS,
    rate        = RATE,
    input       = True,
    output      = True)
    
########### SOUND5 WRITE AND READ ##################

p5 = pyaudio.PyAudio()

w5 = wave.open('Sound50.wav', 'w') #volume 38% to hear and play

w5.setnchannels(1)			# one channels 
w5.setsampwidth(2)			# two bytes per sample (16 bits per sample)
w5.setframerate(Fs)			# samples per second

stream5 = p5.open(
    format      = p5.get_format_from_width(WIDTH),
    channels    = CHANNELS,
    rate        = RATE,
    input       = True,
    output      = True)

#########################################################################################
BLOCKLEN2 = 1024     # block length in samples

K = int( DURATION * RATE / BLOCKLEN2 )   # Number of blocks


while Key:
    root.update()
    i+=1

    if i > 1:
        i = 0
    
    if change > 0:
        change = 0
    
    if change < 0:
        change = 0

    if line == dpk:
        if one[0] == sound:
            x[k1] = 10000.0
            print("thumb")
            x = x

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
        
    if line2 == dpk:
        if two[0] == sound:
            x2[0] = 10000.0
            print("index")
            x += x2

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
   
    if line3 == dpk:
        if three[0] == sound:
            x3[0] = 10000.0
            print("middle")
            x += x3
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

    if line4 == dpk:
        if four[0] == sound:
            x4[0] = 10000.0
            print("ring")
            x += x4
        
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

    if line5 == dpk:
        if five[0] == sound:
            x5[0] = 10000.0
            print("pinky")
            x += x5

        if change == 0:
            a = a5
            b = b5
        
        if change == 1:
            a_2 = a5_2
            b_2 = b5_2

        if change == 2:
            a_3 = a5_3
            b_3 = b5_3

        if change == 1:
            a_4 = a5_4
            b_4 = b5_4

        if change == 2:
            a_5 = a5_5
            b_5 = b5_5

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

    x[0] = 0.0
    x2[0] = 0.0
    x3[0] = 0.0
    x4[0] = 0.0
    x5[0] = 0.0

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

    
    line  = check_data()  [:-1]
    line2 = check_data2() [:-1]
    line3 = check_data3() [:-1]
    line4 = check_data4() [:-1]
    line5 = check_data5() [:-1]
        
    if line: 
        one[i] = line

    if line2:
        
        two[i] = line2

    if line3:
       
        three[i] = line3

    if line4:
       
        four[i] = line4

    if line5:
      
        five[i] = line5
    
   # print("...") 


global save
save = 0

while record:
    root.update()
    CONTINUE = False
    if line == dpk:
        if one[0] == sound: 
            print("Record For Thumb")
            print("Sound Effect 1: Robot ")
            print('* Start')

            save = 1

  
    if line2 == dpk:
            if two[0] == sound: 
                print("Record For Index")
                print("Sound Effect 2: ")
                print('* Start')

                save = 2
   
    if line3 == dpk:
        #if three[0] == sound: 
            print("Record For Middle")
            print("Sound Effect 3: ")
            print('* Start')

            save = 3

        
    if line4 == dpk:
        #if four[0] == sound: 
            print("Record For Ring")
            print("Sound Effect 3: ")
            print('* Start')
            
            save = 4


    if line5 == dpk:
        #if five[0] == sound: 
            print("Record For Pinky")
            print("Sound Effect 3: ")
            print('* Start')

            save = 5

    print(save)

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

    
    if save == 2:
        for j in range(K):
            #input_bytes2 = stream2.read(BLOCKLEN2)
            input_bytes2_ = stream2.read(BLOCKLEN2, exception_on_overflow = False)
            signal_block = struct.unpack('h' * BLOCKLEN2, input_bytes2_)  # Convert
    
            f, t, Zxx1 = signal.stft(signal_block, fs = 1024, nperseg = 512 )
            Zxx = np.abs(Zxx1)
            t, y = signal.istft(Zxx, fs = 1024, nperseg = 512 )
        
            y0 = y.astype(int) 
            output_value = np.clip(10*y0, -MAX, MAX)

            output_bytes = struct.pack('h'* BLOCKLEN2, *y0)    # Convert output value to binary string 
            stream2.write(output_bytes)                        # Write binary string to audio stream
            
            w2.writeframes(output_bytes)  

        print('* DONE 2')
        save = 0


    if save == 3:
        for j in range(K):
            #input_bytes3 = stream3.read(BLOCKLEN2)
            input_bytes3_ = stream3.read(BLOCKLEN2, exception_on_overflow = False)
            signal_block = struct.unpack('h' * BLOCKLEN2, input_bytes3_)  # Convert
    
            f, t, Zxx1 = signal.stft(signal_block, fs = 1024, nperseg = 512 )
            Zxx = np.abs(Zxx1)
            t, y = signal.istft(Zxx, fs = 1024, nperseg = 512 )
        
            y0 = y.astype(int) 
            output_value = np.clip(5*y0, -MAX, MAX)

            output_bytes = struct.pack('h'* BLOCKLEN2, *y0)    # Convert output value to binary string 
            stream3.write(output_bytes)                        # Write binary string to audio stream
            
            w3.writeframes(output_bytes) 

        print('* DONE 3')     
        save = 0



    if save == 4:
        for j in range(K):
            #input_bytes4 = stream4.read(BLOCKLEN2)
            input_bytes4_ = stream4.read(BLOCKLEN2, exception_on_overflow = False)
            signal_block = struct.unpack('h' * BLOCKLEN2, input_bytes4_)  # Convert
    
            f, t, Zxx1 = signal.stft(signal_block, fs = 1024, nperseg = 512 )
            Zxx = np.abs(Zxx1)
            t, y = signal.istft(Zxx, fs = 1024, nperseg = 512 )
        
            y0 = y.astype(int) 
            output_value = np.clip(20*y0, -MAX, MAX)

            output_bytes = struct.pack('h'* BLOCKLEN2, *y0)    # Convert output value to binary string 
            stream4.write(output_bytes)                        # Write binary string to audio stream
        
            w4.writeframes(output_bytes) 

        print('* DONE 4')
        save = 0
 
    if save == 5:
        for j in range(K):
            #input_bytes5 = stream5.read(BLOCKLEN2)
            input_bytes5_ = stream5.read(BLOCKLEN2, exception_on_overflow = False)
            signal_block = struct.unpack('h' * BLOCKLEN2, input_bytes5_)  # Convert
    
            f, t, Zxx1 = signal.stft(signal_block, fs = 1024, nperseg = 512 )
            Zxx = np.abs(Zxx1)
            t, y = signal.istft(Zxx, fs = 1024, nperseg = 512 )
        
            y0 = y.astype(int) 
            output_value = np.clip(0.5*y0, -MAX, MAX)

            output_bytes = struct.pack('h'* BLOCKLEN2, *y0)    # Convert output value to binary string 
            stream5.write(output_bytes)                        # Write binary string to audio stream
        
            w5.writeframes(output_bytes) 

        print('* DONE 5')
        save = 0
  

    line  = check_data()  [:-1]
    line2 = check_data2() [:-1]
    line3 = check_data3() [:-1]
    line4 = check_data4() [:-1]
    line5 = check_data5() [:-1]
      
    if line: 
        #print("line")
        #print(line)
        one[i] = line

    if line2:
        #print("line2")
        #print(line2)
        two[i] = line2

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
        five[i] = line5

    print("...") 
print('* Done Recording. ')
save = 0
record = False
#CONTINUE = True

stream1.stop_stream()
stream1.close()
p1.terminate()

stream2.stop_stream()
stream2.close()
p2.terminate()

stream3.stop_stream()
stream3.close()
p3.terminate()

stream4.stop_stream()
stream4.close()
p4.terminate()

stream5.stop_stream()
stream5.close()
p5.terminate()

      
print('* Done.')
stream.stop_stream()
stream.close()
p.terminate()
