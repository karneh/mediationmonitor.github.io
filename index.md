## Overview

Our project idea is to be able to detect breathing and heartbeat patterns while a person meditates. Given that meditation is very focused on the breath, our goal is to help detect if someone is focused or not focused during their meditation session based on how they are breathing. We plan to use our smartphone as the sensor, and we will collect data by finding an optimal placement on our chest to measure frequency of breathing and heartbeat.

We find this useful because we can help people new to meditation re-focus themselves during the session.

## <a id="Data_Collection"></a> Data Collection Methods ##

### Sample Rate
We chose a sample rate by doing XYZ...

### Setup
Insert pictures here, as well as a diagram for the phone orientation.

<img src="https://github.com/karneh/mediationmonitor/blob/gh-pages/Nathan_Side.jpg" width="300"/> <img src="https://github.com/karneh/mediationmonitor/blob/gh-pages/Nathan_Top.jpg" height="225"/>

## Analytical Method
Our analysis includes the following steps:
1. Trim data (accel) --> subtract control
2. fft
3. fftshift
4. Filter out unecessary frequencies (using band pass filter, how we determine frequency range)
5. Plot

## Results
Show our patterns for heart rate + breath rate.

## Limitations

This project is far from perfect and there are a variety of things that could be done to improve the utility/accuracy of our monitor. These limitations can be broken into several categories

##### Sensor Placement and choice
- Using an accelerometer as a measurement device requires consistent orientation of the device. For this reason our datasets are likely different between samples.
- Accelerometers are by no means the most effective way to measure heartbeat or breath-rate. (a pulse oximeter and computer vision might yield better results respectively)
- Components of the motion from breathing are present in all 3 axes. This makes computations either more difficult, or more error-prone
- Other frequencies (yawning, swallowing, fidgeting will also be picked up by the accelerometer)

##### Data Processing
- Fourier transforms of time chunks yield presence of certain frequencies (not every frequency). This may makes our breath rate and heart rate detection difficult as we are choosing one frequency with the highest amplitude out of a small number (see figures)
- Raw accelerometer data is unfiltered
- Limited in sampling frequency between 10-100hz

##### Nature of the problem
- People breathe very differently. Breathing through your nose vs mouth or diaphragm yields very different accelerometer plots.
- The frequencies we are measuring change over time, we attempt to combat this by looking at small time steps
- Breathing frequencies are quite low and variable which requires us to have large samples
- Breath rate can be controlled more than heart rate, which yields inconsistent signals 
- While meditating heart rate generally slows and breath rate should become lower and less “aggressive”. This makes the input signal smaller and more difficult to read.


## Next steps
Many of the above limitations/problems are caused by the use of an accelerometer to measure both heartbeat and breath rate. The other limitations tend to fall into the fact that both heart rate and breath rate can vary over time.

Given this, it would be practical to measure both heart-rate and breath rate in another way. Our group recommends using a pulse-oximeter to measure heartbeat and a temperature sensor to measure exhales. Using this array of sensors would remove any use of acceleration data. This would make the signal processing much simpler. Both heart rate and breath rate could be detected in real time and a moving average could be used to estimate the current value.

If the project were to continue using an accelerometer as the primary sensor then the current processes used could be improved. One problem identified is the lack of discrete frequencies calculated in the ranges that were of interest. To obtain a better understanding of this range the size of the FFT matrix calculated by the fft function could be increased. This would yield a denser spread of frequencies within the desired ranges. 

In terms of product creation, this software is ready for a beta launch! Refactoring our code into a smartphone app is the final step. This app would be able to parse an entire meditation session into smaller time chunks and then allow the user to look back at portions of their session that were above or below their recommended breathing and heart rates.

