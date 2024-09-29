### Robotization Filter
![image](https://github.com/user-attachments/assets/0644e009-2694-4793-aff8-a2c3050a2160)
![image](https://github.com/user-attachments/assets/ab3088c4-5650-4549-8125-4be790027ce6)

#### Steps:
##### Open a audio stream to take in data
##### Read audio stream in blocks in a loop
##### Unpack the signal to alter the data (converting binary string to array_like time series of values)
##### stft() to get frequency information and change it in next step
##### abs() to remove negatives to implement the effect
##### istft() to turn data back into array like data that can be packed in the next step
##### Put through clipping to make sure the data does not exceed certain values/limits
##### Pack the data (back to binary string)
##### Output data to save/write to file

***
###### FFT: Fourier analysis converts a signal from its original domain (often time or space) to a representation in the frequency domain and vice versa.

###### SFFT: a Fourier-related transform used to determine the frequency and phase content of local sections of a signal as it changes over time. It evaluates the Fourier transform over a short time window and is useful for real-time signal processing.
