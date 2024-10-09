## Sections of Real Time Signal Processing Code in Python
***
### Playing Keyboard Sound
#### To create the keyboard sound effects a second order infinite impulse response filter and clipping of the data was implemented. 

#### Setting up second order IIR filter
![image](https://github.com/user-attachments/assets/7270562c-9469-4e08-bdfc-e20e87333142)
![image](https://github.com/user-attachments/assets/1fa8693d-542e-4b4b-9368-cc8370099ec7)

#### Getting the filter coefficients
![image](https://github.com/user-attachments/assets/aeaed3b2-f56b-4921-afe1-96748d90c463)

##### With the desired frequency from each key sound obtain the pole radius and angle (pole and zero information) to obtain the filter coefficients that will change the filter and therefore the data, based on the desired frequency

##### Poles and zeros relate to the frequency response.
###### Angle: (in radians) around the unit circle (from 0 to 2π) corresponds to angular frequency
###### Radius: (from 0 to ∞) corresponds to the strength of the pole or zero at that frequency


##### Set up filter order
##### Store sound being created
##### Opening audio output stream so the audio can be created, stored and/or played
![image](https://github.com/user-attachments/assets/abc6c2a9-4fad-40f0-b5ff-f9210788c342)

#### Second order IIR filter
![image](https://github.com/user-attachments/assets/abf52ebf-a254-4ae1-8b24-d9c20bc8be4a)
