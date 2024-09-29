## Sections of Real Time Signal Processing Code in Python
***
### Playing Keyboard Sound
#### To create the keyboard sound effects a second order infinite impulse response filter and clipping of the data was implemented. 
#### Setting up second order IIR filter
![image](https://github.com/user-attachments/assets/5c887339-dfe8-442b-be74-51b86d22a6e3)

#### Getting the filter coefficients
![image](https://github.com/user-attachments/assets/62b78268-fe98-4a74-abc5-6f10dedee140)
with the desired frequency I wanted from each key sound. i obtained the pole radius and angle (pole and zero information) to obtain the filter coefficients that will change the filter and therefore the data, based on the desired frequency

poles and zeros relate to the frequency response ->
the angle -> (in radians) around the unit circle (from 0 to 2π) corresponds to angular frequency
the radius -> (from 0 to ∞) corresponds to the strength of the pole or zero at that frequency


Setting up filter order, and place to store sound being created
Opening audio output stream so the audio can be created, stored and/or played
![image](https://github.com/user-attachments/assets/abc6c2a9-4fad-40f0-b5ff-f9210788c342)

#### Second order IIR filter
![image](https://github.com/user-attachments/assets/abf52ebf-a254-4ae1-8b24-d9c20bc8be4a)
***
***
### Robotization Filter
![image](https://github.com/user-attachments/assets/0644e009-2694-4793-aff8-a2c3050a2160)
![image](https://github.com/user-attachments/assets/ab3088c4-5650-4549-8125-4be790027ce6)

##### Open a audio stream to take in data
##### Read audio stream in blocks in a loop
##### Unpack the signal to alter the data (converting binary string to array_like time series of values)
##### stft() to get frequency information and change it in next step
##### abs() to remove negatives to implement the effect
##### istft() to turn data back into array like data that can be packed in the next step
##### Put through clipping to make sure the data does not exceed certain values/limits
##### Pack the data (back to binary string)
##### Output data to save/write to file

###### FFT -> Fourier analysis converts a signal from its original domain (often time or space) to a representation in the frequency domain and vice versa.

###### SFFT -> a Fourier-related transform used to determine the frequency and phase content of local sections of a signal as it changes over time. It evaluates the Fourier transform over a short time window.
***

