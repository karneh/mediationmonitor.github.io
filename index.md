## Abstract

Our project goal is to be able to detect breathing and heartbeat patterns while a person meditates. 
Given that meditation is very focused on the breath, 
**our goal is to help detect if someone is focused or not focused during their meditation session based on how they are breathing**. 
We plan to use our smartphone as the sensor, and we will collect data by finding an optimal placement on our chest to measure frequency of breathing and heartbeat.

<details style="padding-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Background</summary>
There are not many existing quantitative measures of meditation that are reliable. Electroencephalogram (EEG), the detection of electrical activity in the brain, and heart rate variability (HRV) are two existing methods, though they are both responses that take a long time to detect patterns from. Breath rate, however, is a measure that is controlled by meditation and can have changes detected over a short period of time.
</details>

<details style="padding-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Use Case</summary>
We believe that we can help people new to meditation better understand their meditative patterns through breath rate detection and heart rate detection. Our app would be able to help those new to meditation re-focus themselves during the session if it detects their breath rate and heart rate are not stable (meaning they are not focused).
</details>

___

## <a id="Data_Collection"></a> Data Collection Method ##

We collected various one minute and five minute samples of someone lying down and meditating, alongside one minute and five minute control samples of the phone lying in the same orientation and location. The purpose of our control samples were so that we could subtract any noise present in our meditation samples.

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Sensor Choice</summary>
  We chose to use a Pixel phone's accelerometer as our sensor. This selection was made primarily because we knew the focus of our project was on acceleration data. We also knew that for those new to meditation, a phone is often already present and used as a meditation guide. As such, we decided a phone was the most accessible and reliable sensor for the purposes of this project. 
  
  The phone was oriented with the following axes:

<img src="images/phone_orien.png" width="300"/>
</details>

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Set Up</summary>
  
  <img src="images/nathan_side.jpg" width="300"/> <img src="images/nathan_top.jpg" height="225"/> 
  
  To collect data, we simply placed our sensor (the phone) on the chest of the person whose data is being collected. We found that the chest was the optimal placement compared to other places on the body, since it was where our sensor could register heartbeat and breathing.
</details>

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Sample Rate</summary>
  Given the fact that heartbeat is about 0.66 - 1.33 Hz and breath rate is about 0.16 - 0.25 Hz, we wanted a sample rate greater than those ranges to make sure it could pick up both patterns. We started off with data sampled at 10 Hz and then transitioned to 50 Hz after discovering (through trial and error) that data sampled at 50 Hz gave us more data points to analyze.
</details>

___

## Motion Model
To observe and measure meditation with an accelerometer, we must have a base understanding of the various motions occurring. In our experiment we are interested in two motions: breathing and heartbeats. 

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;">Heartbeat Motion</summary>
Heartbeats are easily measured by one’s own fingers. We can sense the “pulse” of increased blood flow in our veins. The accelerometer seeks to do something extremely similar. If the accelerometer was positioned normal to the heartbeat (so that the movement due to heartbeat was in 1 axis) we would expect the phone to accelerate up and down due to the change of blood flow.

One cycle, or one heartbeat, should correspond to one period of our acceleration normal to the heartbeat. From our research we know that the heart rate of individuals in meditation to be between 50-100 bpm[SOURCE NUM]. This means that we can expect to see this 50-100bpm (.8-1.6hz) signal present in our acceleration data.

The above analysis is based on the fact that the phone is positioned normal to the observed motion. Our sensor choice does not gurantee this. For this reason, we expect to see accelerations matching these frequencies in all axes.
</details>

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Breathing Motion</summary>
Breathing is arguably more complex motion than the heartbeat, one reason is since it can be controlled by the individual. Our experiment seeks to measure the rise and fall of the subject’s chest while breathing. This motion should also be cyclical in nature. 

We can expect an acceleration while inhaling and corresponding acceleration when exhaling. This means that we should large amounts of the frequencies with the breathing rate (.1-.5hz)[SOURCE] Once again this motion should be primarily recorded in one axis. However, the phone will not be placed perfectly on the body so we can expect to see corresponding signals/frequencies in all axes.

The motion of breathing causes the phone to move much more than a heartbeat. This will cause the sensor to potentially move throughout measurement collection. This could introduce error into our data. 

We expect a raw acceleration plot to look something like this:

<img src="images/sim_time.jpg" width="400"/>

***Generated wave with .95hz(heartbeat) and .2hz(breathrate) sine waves ***
</details>

<details style="margin-bottom:8px; border:none;">
  <summary style="font-size:18px; color:#159957;" onfocus = "this.style.outline = 'none'">Design decisions based on motion</summary>
  
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

#### Method Validation
To validate our method we performed a first pass analysis on a constructed signal with the frequencies of interest.
Below is a plot of the signal generated in the time domain. This signal has both a .95 hz and 0.2 hz signal in the dataset. These two frequencies represent a heart and breath rate respectively. 

<img src="images/sim_time.png" width="400"/> 

***Time domain plot of simulated signal w/noise (.2hz and .95 hz signals)***

This signal is then converted into the frequency showing using Matlab’s FFT (Fast Fourier Transform) function ()[INSERT LINK]. This indicates how much of a certain frequency is present in a sample. Below is the figure generated from the FFT function. This signal has been shifted into the frequency domain (hz).

<img src="images/sim_freq.png" width="400"/> 

***Frequency domain plot of simulated signal***

This plot informs us of several things. We do indeed see the presence of the frequencies of interest. Interestingly, there is no maximum amplitude centered around the 0.95hz value. Instead it appears that there are sligh spikes at .9 and 1 hz. This case shows the shortcoming of our process. In the case where a frequency is present in our signal but not aligned with the frequencies used in the fft function the “real” frequency can be masked.

One idea we had was to remedy this problem by using the additional parameter that controls the size of the matrix used to calculate the fft of our signal (https://www.mathworks.com/help/matlab/ref/fft.html#f83-998360-n)[https://www.mathworks.com/help/matlab/ref/fft.html#f83-998360-n]. This parameter could generate more points between a given range in our plot and allow us to look at a more dense range of frequencies.

<img src="images/sim_freq.png" width="00"/> <img src="images/sim_freq_double.png" width="400"/> 

***Above are two plots showing the same frequency domain plot using fft(x, length(x)) and fft(x, 2*length(x)) respectively***

These graphics show that while this doesn’t create the ideal behavior(other noise in the signal seems to be increased), it does create spikes centered around the expected values at .2 and .95hz. Unfortunately, this yields other spikes in non-interesting frequencies. Adjusting the __N__ parameter is worth testing, however, from this data a simple weighted average of the regions of interest would yield the same peak.

Another problem we have identified is the fact that the frequencies will change over time.

In order to understand what our fft would do to an input signal with a varying frequency 
TBC

___

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

___

## Next steps
Many of the above limitations/problems are caused by the use of an accelerometer to measure both heartbeat and breath rate. The other limitations tend to fall into the fact that both heart rate and breath rate can vary over time.

Given this, it would be practical to measure both heart-rate and breath rate in another way. Our group recommends using a pulse-oximeter to measure heartbeat and a temperature sensor to measure exhales. Using this array of sensors would remove any use of acceleration data. This would make the signal processing much simpler. Both heart rate and breath rate could be detected in real time and a moving average could be used to estimate the current value.

If the project were to continue using an accelerometer as the primary sensor then the current processes used could be improved. One problem identified is the lack of discrete frequencies calculated in the ranges that were of interest. To obtain a better understanding of this range the size of the FFT matrix calculated by the fft function could be increased. This would yield a denser spread of frequencies within the desired ranges. 

In terms of product creation, this software is ready for a beta launch! Refactoring our code into a smartphone app is the final step. This app would be able to parse an entire meditation session into smaller time chunks and then allow the user to look back at portions of their session that were above or below their recommended breathing and heart rates.

___

## Sources

Laskowski, Edward. “Heart Rate: What’s Normal?” Mayo Clinic, 2 Oct. 2020, www.mayoclinic.org/healthy-lifestyle/fitness/expert-answers/heart-rate/faq-20057979.

“Mindful Breathing (Greater Good in Action).” Greater Good in Action, ggia.berkeley.edu/practice/mindful_breathing. Accessed 6 Dec. 2020.

Soni, Rahul, and Manivannan Muniyandi. “Breath Rate Variability: A Novel Measure to Study the Meditation Effects.” International Journal of Yoga, Medknow Publications & Media Pvt Ltd, Jan. 2019, www.ncbi.nlm.nih.gov/pmc/articles/PMC6329220/.

“Vital Signs (Body Temperature, Pulse Rate, Respiration Rate, Blood Pressure).” Johns Hopkins Medicine, www.hopkinsmedicine.org/health/conditions-and-diseases/vital-signs-body-temperature-pulse-rate-respiration-rate-blood-pressure. Accessed 6 Dec. 2020.
