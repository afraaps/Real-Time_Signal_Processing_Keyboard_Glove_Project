The python code uses real time signal processing methods to record and create audio outputs for the signals given by the hardware I designed to monitor hand movement. The sensor signals are sent from a microncontroller run on Arduino code and sent with a WIFI protocol (not shown in this project) to Python Code to a PC for signal processing and audio generation. Orginaly the filters and python codes were apart of one larger code with additional GUI code to allow for switching between filters, but was seperated here for the purpose of explaining everything in a simpler manner.

Basic overview of methods in this project include a robotization filter applied to recorded audio in real time:
  * block processing of the signal
  * short time fourier transform of the data 
  * altering the data additionally 
     * using the abs() function to remove negative frequencies
     * inverse short time fourier transform
   
Basic overview of methods in this project include a second order IIR filter to play keyboard sounds:
  * impulse signal and filter coefficients application to the signal
     * filter coefficients calculated by the desired frequency, radius, and angle to get the desired transfer function's poles and zeros

###### Note: This project was a part of NYU Tandon School of Engineering's Real Time Signal Processing end of term project taught by Professor Ivan Selesnick and uses the signal processing methods taught in this course.

![IMG_0180](https://github.com/user-attachments/assets/d7037abe-bf47-411c-ad44-930f41106ef3)

