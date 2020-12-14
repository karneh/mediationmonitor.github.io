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
1. Trim the very beginning and very end of the samples to remove anomalies.
2. Split the data into smaller time chunks in order to examine how metrics like breathing rate and heart rate vary throughout the meditation session.
3. Shift the FFT 
4. Filtered out amplitudes from control data that was taken along the same axis at the same place. 
5. Utilize a band pass filter to decrease the amplitude of frequencies we are not interested in. To choose the frequency ranges we looked up data on breath rates and heart rates. 
6. Overplot data and observe how the frequencies associated with heart rate and breath rate changed over time. 


## Results
Show our patterns for heart rate + breath rate.

## Limitations & Next Steps
1. Filtering
2. Short samples
3. Not accurate breath and heart rates.
4. Our phone orientation
