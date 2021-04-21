## Analysis-functionality to be tested(Kindly refer the document "Flow of the implementation" in order to understand the system flow which needs to be tested)

This section lists the Analysis for which _tests_ must be written.

**INPUT**
Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

**Analysis** must be done on this csv file to find the following:

1. Minimum
2. Maximum
3. Count of breaches (how many times did it cross the threshold in a month?)
4. Record trends (date & time when the reading was continuously increasing for 30 minutes)

**OUTPUT**
1. PDF report of the analysis must be stored every week.
2. Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file.
2. Read access for the csv file in order to perform the analytics.
3. Threshold values for the battery parameters should be available in order to calculate the no. of breaches.
4. PDF utility should be supported.
5. Alert/Notification system should be known(e.g: if it's EMAIL_ALERT, CONTOLLER_ALERT or simply a CONSOLE_ALERT)


### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       |     No        | We do not test the accuracy of data
Computation of maximum      |     Yes       | This is part of the software being developed
Off-the-shelf PDF converter |     Yes       | Data should be read from .csv file, analytics needs to be performed and stored in PDF format in the server
Counting the breaches       |     Yes       | When battery parameters exceeds their threshold values it needs to be accounted as a breach
Detecting trends            |     Yes       | Whenever the reading is increasing continuously for 30 mins date and time needs to be recorded
Notification utility        |     Yes       | When PDF report is generated recipient should be notified

### Test Cases


1. Write minimum and maximum to the PDF from a .csv file containing positive and negative readings.
2. If there are no breaches then the default count of breaches should be returned which is equivalent to '0'(zero).
3. I/P file should be of .csv format. No other format I/P file should be allowed. If I/P format is other than .csv then it should return "Invalid I/P file format".
4. Write "Invalid input" to the PDF when the .csv doesn't contain expected data.
5. Battery telemetrics file(.csv file) should be available on the server. If not then the recipient should get a notification that the file doesn't exist follwed by immediate    fetching of the .csv from the local server.
6. All the mandatory fields and columns must be checked on which analystics to be performed(i.e., name of the coulmns,their sequence and no. of columns in the .csv file should be same throughout the monthly files). If not then it should specify the file name having invalid format(e.g: 'Mar.csv has invalid format').
7. Duplicate entries should not be allowed to avoid inaccuracy in the analysis generated. If exists then it should be deleted automatically.
8. Check if all the analytics is performed or not(min,max, count of breaches and trends). If not then specify the missing analytics.
9. Check if PDF is generated and stored on weekly basis in the server. If not notify the registered id.
10. Check if the recipient to whom the notification is to be sent is configured or not. if not then ask user to mention the recipient.
11. Check if we have the utility to send notification to the registered recipient.
12. Check if .csv's are generated and stored on monthly basis or not. If not then alert should be sent to default registered id stating that 'week1.pdf' is missing.
13. Check if data exist specific to the month in the I/P **MONTH_NAME.csv** file e.g: In the March.csv file we should not have an entry of februrary(Date validation should be done on the basis of date and the month of the .csv file). If such entry exist then it should not be allowed and user must be informed.
14. Check if there is any limit on the size of the file(in terms of no. of bytes) or if excess of data causes stack overflow.
15. Alert/Notification system should be configured and tested with 'dummy' input.

**NOTE: Manual regarding the functioning of the system should be available(**Flow of the implementation.docx**)**

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                                               | Faked/mocked part
|--------------------------|--------------|------------------------------------------------------|--------------------
Read input from server     | .csv file     | internal data-structure                             | Fake the server store
Validate input             | .csv data     | valid / invalid                                     | None - it's a pure function
Notify report availability | .pdf file     |  True/False                                         | Fake the notification
Report inaccessible server | .csv file     |  No access to the server                            | Fake the server
Find minimum and maximum   | .csv data     |  Return min and max value                           | None - it's a pure function
Detect trend               | .csv data     |  Date & Time, reading                               | None - it's a pure function
Write to PDF               | .csv data     |  Min, Max, Count_Of_breaches, Date,Time & Reading   | Fake PDF utility
