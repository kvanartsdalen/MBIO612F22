Daily Fishing Effort at 10th Degree Resolution by MMSI

Description

This dataset contains the original release of the Global Fishing Watch fishing effort data and includes fishing effort by MMSI binned into grid cells 0.1 degrees on a side, and measured in units of hours. The time is calculated by assigning an amount of time to each AIS detection (which is half the time to the previous plus half the time to the next AIS position). To get information on each MMSI, see Global Fishing Watch data on fishing vessels.


Fishing effort data are available for download in the following formats:
  - mmsi-daily-csvs-10-v1-YYYY.zip: Zip file containing a folder of daily csv files for the specified year in the format YYYY-MM-DD.csv.


Table Schema
 - X: observation number
 - latitude: latitude coordinate bin (the southern edge of the grid cell, in 10ths of a degree -- 101 is the grid cell with a southern edge at 10.1 degrees north. Divide by 10 to convert to decimal degrees)
 - longitude: longitude coordinate bin (the western edge of the grid cell, in 10ths of a degree -- 101 is the grid cell with a western edge at 10.1 degrees east. Divide by 10 to convert to decimal degrees)
 - mmsi: unique AIS identifier of vessel (Maritime Mobile Service Identity)
 - fishing_hours: hours that vessels were fishing in this gridcell on this day
 - month: month of data
 - day: day of data
 - year: year of data for annual csv
