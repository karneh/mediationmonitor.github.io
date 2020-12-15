## Abstract

Our project goal is to be able to detect breathing and heartbeat patterns while a person meditates. 
Given that meditation is very focused on the breath, 
**our goal is to help detect if someone is focused or not focused during their meditation session based on how they are breathing**. 
We plan to use our smartphone as the sensor, and we will collect data by finding an optimal placement on our chest to measure frequency of breathing and heartbeat.

#### Background

There are not many existing quantitative measures of meditation that are reliable. Electroencephalogram (EEG), the detection of electrical activity in the brain, and heart rate variability (HRV) are two existing methods, though they are both responses that take a long time to detect patterns from. Breath rate, however, is a measure that is controlled by meditation and can have changes detected over a short period of time.

#### Use Case

We believe that we can help people new to meditation better understand their meditative patterns through breath rate detection and heart rate detection. Our app would be able to help those new to meditation re-focus themselves during the session if it detects their breath rate and heart rate are not stable (meaning they are not focused).

___

## <a id="Data_Collection"></a> Data Collection Method ##
*You should explain how your model informed your data collection, and what, if any, modifications were made to the model following experimentation. The proof-of-concept should include a selection of sensors that are appropriate for the specific application. The experiment(s) should demonstrate some aspect of how the product would work in reality. Next steps for the proof-of-concept (e.g. additional experiments, changes to the design) should be well articulated.*

We collected various one minute and five minute samples of someone lying down and meditating, alongside one minute and five minute control samples of the phone lying in the same orientation and location.

<img src="images/nathan_side.jpg" width="300"/> <img src="images/nathan_top.jpg" height="225"/> 

The sensor, a Pixel phone, was oriented with the following axes:

<img src="images/phone_orien.png" width="300"/>

#### Sample Rate
We chose a sample rate by doing XYZ...

___

## Motion Model
To observe and measure meditation with an accelerometer, we must have a base understanding of the various motions occurring. In our experiment we are interested in two motions: breathing and heartbeats. 

#### Heartbeat Motion
Heartbeats are easily measured by one’s own fingers. We can sense the “pulse” of increased blood flow in our veins. The accelerometer seeks to do something extremely similar. If the accelerometer was positioned normal to the heartbeat (so that the movement due to heartbeat was in 1 axis) we would expect the phone to accelerate up and down due to the change of blood flow.

One cycle, or one heartbeat, should correspond to one period of our acceleration normal to the heartbeat. From our research we know that the heart rate of individuals in meditation to be between 50-100 bpm[SOURCE NUM]. This means that we can expect to see this 50-100bpm (.8-1.6hz) signal present in our acceleration data.

The above analysis is based on the fact that the phone is positioned normal to the observed motion. Our sensor choice does not allow this to be true. For this reason, we expect to see accelerations matching these frequencies in all axes.

INSERT EXPECTED PLOT HERE

#### Breathing Motion
Breathing is arguably more complex motion than the heartbeat, one reason is since it can be controlled by the individual. Our experiment seeks to measure the rise and fall of the subject’s chest while breathing. This motion should also be cyclical in nature. 

We can expect an acceleration while inhaling and corresponding acceleration when exhaling. This means that we should large amounts of the frequencies with the breathing rate (.1-.5hz)[SOURCE] Once again this motion should be primarily recorded in one axis. However, the phone will not be placed perfectly on the body so we can expect to see corresponding signals/frequencies in all axes.

The motion of breathing causes the phone to move much more than a heartbeat. This will cause the sensor to potentially move throughout measurement collection. This could introduce error into our data. 

#### Design decisions based on motion
<details>
  <summary>Click to expand!</summary>
  
We chose to position the accelerometer directly overtop the heart in hopes of being able to capture the heartbeat (the accelerations from the heartbeat will be of much smaller magnitude than breathing). This placement will allow the monitoring of breath rate at the same time as the heart rate and will minimize other unwanted sensor movements like someone flexing their abs or moving their neck. This position will also have a near zero angular velocity as almost all of the movement is normal to the phone and doesn’t change its rotation around any axes.
</details>

___

## Analytical Method
Our analysis includes the following steps:
1. Trim data (accel) --> subtract control
2. fft
3. fftshift
4. Filter out unecessary frequencies (using band pass filter, how we determine frequency range)
5. Plot

The algorithm(s) for data analysis should demonstrate a clear understanding of Fourier analysis, frequency and time domains, and motion model dynamics. The project website should clearly explain the application of the algorithm to the experimental data through the use of appropriate equations and graphics.

## Results
Show our patterns for heart rate + breath rate.

## Limitations

This project is far from perfect and there are a variety of things that could be done to improve the utility/accuracy of our monitor. These limitations can be broken into several categories

#### Sensor Placement and choice
- Using an accelerometer as a measurement device requires consistent orientation of the device. For this reason our datasets are likely different between samples.
- Accelerometers are by no means the most effective way to measure heartbeat or breath-rate. (a pulse oximeter and computer vision might yield better results respectively)
- Components of the motion from breathing are present in all 3 axes. This makes computations either more difficult, or more error-prone
- Other frequencies (yawning, swallowing, fidgeting will also be picked up by the accelerometer)

#### Data Processing
- Fourier transforms of time chunks yield presence of certain frequencies (not every frequency). This may makes our breath rate and heart rate detection difficult as we are choosing one frequency with the highest amplitude out of a small number (see figures)
- Raw accelerometer data is unfiltered
- Limited in sampling frequency between 10-100hz

#### Nature of the problem
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

