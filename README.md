# spadesAnalytics-activityCount
Activity count algorithm is implemented to convert the raw acceleration data to activity count. The activity count results are comparable to those in Actigraph software.

Usage
-----
```ShellSession
[Hadoop Path] [jar] [Jar File Name] [Class Path for Algorithm in Jar] [-c] [Cross Participants] [-f] [File System] [-j] [Runnable Jar File Path] [-a] [Class Path for Algorithm in Jar] [-i] [Data Input Path]
```

* [Local directory to jar file]: The local directory for jar file 
   * Example:  output/SpadesAnalytics.jar (here SpadesAnalytics.jar is a runnable jar file in the local computer)
* [com.qmedic.spades.task.SpadesSubmit]: the class to submit the job to EMR 
* [s3n directory to jar file]: The cloud directory for jar file
   * Example: spades-data/development/user/SpadesAnalytics.jar (SpadesAnalytics.jar is stored in the directory “spades-data/development/user” of s3)
* [class name in jar file of s3n]: The runnable class in the jar file in S3
   * Example: com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean(The class to run for converting raw acceleration data to activity count)
* [data input path]: Cloud directory for source data
   * Note: Its format will be discussed in the section “Data Input Format”
   * Example: spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*
* [key value assigned by AWS]: Key value assigned by AWS
   * Note: 20  alphabeta
   * Example: "yourAccessKey"
* [secret value assigned by AWS]: secret value assigned by AWS
   * Note: 40 char
   * Example: "yourSecretKey"
* [parameters to be passed to runnable class]: parameters to be passed to runnable class
   * Note: Its format will be discussed in the section “Parameter Input Format”

Input
-----
In our framework, the input format for processing data is restricted to the following, a future work item would be to extend it and allow for a variety of flexible formats…

* Input Path Folder Format
   * Must be terminated with a wild card /*
   * The path consists of the following required entries:
      * Bucket Name: (alphanumeric, ‘-’)... e.g. spade-data
      * Release Type: development, production
      * Institution: Stanford
      * Study ID: numeric value for the user within the institution
      * Study Name:  StanfordStudyYouth2
      * Participant ID: acceptable values include (can also be specified as a wild card *)
      * Data source: Alphanumeric + ‘-’...e.g. MasterSynced
   * Optional components of the path that follow include:
      * Year: 4 character representation
      * Month: 2 character representation that goes from 01 to 12
      * Day: 2 character representation that goes from 01 to 31
      * Hour: 2 character representation that goes from 00 to 23
        
   * Allowed paths include the following formats:
spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*

Parameter Input
----------------------
The parameter input format for passing parameters to runnable class is restricted to the following, a future work item would be extended and allow for a variety of flexible formats.

*Format*
```ShellSession
-c [false] -f [s3n] -j [s3n directory to jar file] -a [class name in jar file of s3n] -i [data input path]
```

*Example*
```ShellSession
-c false -f s3n -j spades-data/development/user/SpadesAnalytics.jar -a com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean -i spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*
```
* [false]: Input value for [-c] command which doesn’t allow cross participants
   * Example: false/true
* [s3n]: Input value for [-f] command which runs in s3n file system
   * Example: s3n/hdfs
* [s3n directory to jar file]: Input jar directory in s3n for [-j] command which specifies the runnable jar file
   * Example: spades-data/development/user/SpadesAnalytics.jar 
* [class name in jar file of s3n]: Input s3 class for [-a] command which specifies the runnable class 
   * Example: com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean
* [data input path]: Input source data for [-i] command which specifies the source data
   * Example: spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*

Command Line Example
--------------------
```ShellSession
java -cp output/SpadesAnalytics.jar com.qmedic.spades.task.SpadesSubmit -cluster MapReduce -mode Once -speed 32x -filesystem s3n -jar spades-data/development/user/SpadesAnalytics.jar -classpath com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean -input spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*   -key "yourAccessKey" -secret "yourSecretKey" -duration 4 -cost 20 -p "-c false -f s3n -j spades-data/development/user/SpadesAnalytics.jar -a com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean -i spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*"
```

Output
------
Typically the feature data file will be stored in the metadata directories and will include processed data that can be recomputed from the raw data. The structure of the filename is: 
```ShellSession
[ALGORITHM].[AlgorithmID].[YYYY]-[MM]-[DD]-[hh]-[mm]-[ss]-[mmm]-[P/M][hhmm].csv.gz 
```
* ALGORITHM: Prefix of the file name
* [FEATURE NAME]: Feature name
   * Example: MetaData-ActivityCountClean-2015-10-29-10-50-21-021

*Output Content*
For example, the first two lines in the activity count.csv file could look like this:
```ShellSession
HEADER_TIME_STAMP, X_ACTIVITY_COUNT,Y_ACTIVITY_COUNT,Z_ACTIVITY_COUNT,VM_ACTIVITY_COUNT
2013/11/20 00:00:00.000, 0, 0, 0, 0
```
* HEADER_TIME_STAMP: The start timestamp for activity count (here activity count is calculated in 1 minute)
* [X_ACTIVITY_COUNT]: Value of activity count based on the x-axis acceleration data
* [Y_ACTIVITY_COUNT]: Value of activity count based on the y-axis acceleration data
* [Z_ACTIVITY_COUNT]: Value of activity count based on the z-axis acceleration data
* [VM_ACTIVITY_COUNT]: Vector Magnitude of x,y,z activity count
   * Note:  VM_ACTIVITY_COUNT = sqrt(X_ACTIVITY_COUNT^2 + Y_ACTIVITY_COUNT^2 + Z_ACTIVITY_COUNT^2)

Notes
-----
Here it is assumed that the accelerometer has three axis accelerations.
