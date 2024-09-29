The python code uses real time signal processing methods to record and create audio outputs for the signals given by the hardware I created to monitor hand movement. 

Methods in this code include a robotization filter using:
  * block processing of the signal
  * short time fourier transform of the data 
  * altering the data additionally 
     * using the abs() function to remove negative frequencies
     * inverse short time fourier transform 
