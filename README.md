The python code uses real time signal processing methods to record and create audio outputs for the signals given by the hardware I designed to monitor hand movement. The sensor signals are sent from a microncontroller run on Arduino code and sent with a WIFI protocol (not shown in this project) to Python Code to a PC for signal processing and audio generation. 

Methods in this project include a robotization filter:
  * block processing of the signal
  * short time fourier transform of the data 
  * altering the data additionally 
     * using the abs() function to remove negative frequencies
     * inverse short time fourier transform
   
Methods in this project include a second order IIR filter:
  * block processing of the signal
  * impulse response and filter coefficents application
     * calculated by the desired frequency, radius, and angle to get the desired transfer function coeffiencts (poles and zeros)

###### Note: This project was a part of NYU Tandon School of Engineering's Real Time Signal Processing end of term project taught by Professor Ivan Selesnick and uses the signal processing methods taught in this course.
