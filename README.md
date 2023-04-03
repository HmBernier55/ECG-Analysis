# **ECG Analysis Assignment**
### **_Author:_**
Hunter Bernier
## **_License:_**
MIT License
## **_Purpose:_**
The code within this repository:
1. Reads in ECG data from a csv file
2. Removes data that is either missing, contains non-numeric values, or is NaN
3. Calculates duration, minimum and maximum voltages, number of beats, average heart rate, and the times that beats occur
4. Creates a dictionary with these calculated values
5. Outputs the dictionary to a json file
6. Logs an error when a bad data point is removed
7. Logs a warning when a file has voltage values greater than 300 mV or less than -300 mV
8. Logs information regarding when values are being calculated
## **_How to Run the Program:_**
1. Clone the repository onto your local drive
    * Run `git clone <githubURL>`
2. Create a new virtual environment
    * Run `python -m venv <VirtualEnvironmentName>`
3. Activate the new virtual environment
    * Run `source <VirtualEnvironmentName>/Scripts/activate` 
4. Install the required packages
    * Run `pip install -r requirements.txt`
5. To run the program, type `python ecg_analysis.py` on the command line
## **_How to Use the Program:_**
1. Enter the name of the csv file (without the .csv) as a string into the read_file input within the driver() function
    * The csv file needs to have the format:
        ```
        0,-0.035
        0.003,-0.035
        0.006,-0.035
        0.008,-0.035
        0.011,-0.035
        0.014,-0.035
        ```
    * The values on the left of the comma are time values in seconds and the values on the right of the comma are voltage values in mV
2. Run the program using the command stated in the section above
3. The program will then create a .json file with a dictionary containing duration, min and max voltages, number of beats, average heart rate, and the times that beats occur for the inputted ecg signal
    * The filename for the json file will be: `<Inputted csv filename>.json`
        * For example: If I'm analyzing the ecg signal from `test_data1.csv`, then the json file will be named `test_data1.json`
## **_How a Beat is Identified:_**
* A beat is identified using the scipy package with the command `scipy.signal.find_peaks(<list of voltages>, distance = <number of data points>, prominence = (min, max))`
* Within this command, the user has the ability to assign a distance which is the minimum horizontal distance between neighboring peaks
    * A value of 150 samples was chosen, which means once a peak is identified, the code will not try to identify another peak until 150 samples (or data points) has passed
* The command also allows the user to assign a prominence value for the peaks which is the vertical distance between the peak and its lowest contour line or how much the peak stands out from the surrounding baseline of the signal
    * A minimum prominence of 0.6 and a maximum prominence of None was chosen, which means that a peak will only be identified if the data point is 0.6 mV or higher above the surrounding baseline of the signal
* Using these filters within the find_peaks command, the code is able to identify the maximum peaks of the ecg signal while ignoring the smaller/noisier peaks of the signal
## **_How Average Heart Rate is Calculated:_**
* Once the number of beats is determined from the ecg signal, the average heart rate is calculated from the equation: 
    * `avg_HR = ((number_of_beats)/(duration of ecg signal))*60`
