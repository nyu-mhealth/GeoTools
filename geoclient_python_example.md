#####Python example for geoclient api#####
**Written by Frank Donnelly - Baruch College - GIS Librarian**

I was hoping to clean up the scripts I wrote for using the city's geoclient api but alas, that's fallen by the wayside. The next time I'll revisit it is when the next project pops up in front of me.
 
So, as unpolished and ghastly as it may look, I'm attaching the code and the sample data files in case you're interested in re-purposing it for your own projects. In order to use this, you will have to get an app_id and app_key from the city to use with geoclient (https://developer.cityofnewyork.us/api/geoclient-api), and then insert the credentials in the appropriate places throughout the script. You also have to download and import the python bindings for geoclient: https://github.com/talos/nyc-geoclient. The script was written in Python 2.7.
 
The specific request I had involved rematching stop and frisk records that were missing x and y coordinates. If you place the script - nycgeocode_2.py -  in the same folder as the data files (I've included two samples), you can run the whole thing as this function:
 
  sfcoder(thefile,adnum,street,instreet,crstreet,borough,x,y)
 
Where the parameters are the file name followed by the column or index numbers where each of those attributes are stored. The stop and frisk data was inconsistent from one year to the next, so I have a file (columns.xls) that indicates the proper positions for each year. For example:
 
  sfcoder('2013noxy.csv',95,96,97,98,112,107,108)
 
The script loops through the file and for each record tries to get an ideal match - if it fails, then it tries for the next ideal solution. Ideal is based on the availability of data (there were lots of null attributes for thousands of records) as well as whether a match was found with what was available. The script also incorporates a number of fixes that are particular to the stop and frisk files, and outputs the results of the matching to a new csv. There are comments throughout the code so you can see what's going on. After we completed the whole process, we realized that we could also try and get coordinates for landmarks (like the names of parks).Instead of re-running the whole project, we took the output files from the first process and input them into the second script ncygeocode_3_places.py.
 
Ideally, I wanted to rewrite this code to remove redundancies, combine the two scripts into one, generalize it for use with other data sources for address files, and alter the process so that the code does everything for one record from start to finish before moving on to the next record. Instead, this script does all the matching for every record first, and when they're all done it writes the output all at once. In practice, we found for larger files (more than 10k records) the geoclient would occasionally lock up and crash, and we'd have to redo the entire file over again. As a work-around we split bigger files into smaller ones, but it would be better to match a record and write the output immediately, so we'd at least be able to go back and pick up where it left off following a crash.
 
Anyway, there you have it. Hope you're having a good summer! Make the most of these last few, precious weeks.
 
Best - Frank
