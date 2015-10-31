# spadesAnalytics-activityCount
Activity count algorithm is implemented to convert the raw acceleration data to activity count. The activity count results are comparable to those in Actigraph software. The activity count algorithm is based on the manuscript.

Usage
-----
```ShellSession
[Hadoop Path] [jar] [Jar File Name] [Class Path for Algorithm in Jar] [-c] [Cross Participants] [-f] [File System] [-j] [Runnable Jar File Path] [-a] [Class Path for Algorithm in Jar] [-i] [Data Input Path]
```

* [Hadoop Path]: The path where the hadoop is. 
   * Note: This is only for running the algorithm in local clusters
   * Example:  /usr/local/hadoop/bin/hadoop
* [jar]: Specify it to run a jar file. 
   * Note: This command is fixed to be “jar”
   * Example: jar
* [Jar File Name]: Specify the jar file
   * Note:  
   * Example: SpadesAnalytics.jar
* [Class Path for Algorithm in Jar]: Specify the the class path for algorithm to run in jar
   * Note:
   * Example: com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean
* [-c]: Command for cross participant input
   * Note: should be “-c”
   * Example: -c
* [Cross Participants]: Specify whether to run the algorithm cross participants
   * Note:
   * Example: true
   * Options: true/false
* [-f]: Command for file system input
   * Note: should be “-f”
   * Example: -f
* [File System]: Specify the file system
   * Note: hdfs for local clusters; s3n for amazon s3n service
   * Example: hdfs
   * Options: hdfs/s3n
* [-j]: Command for jar file input
   * Note: should be “-j”
   * Example: -j
   * [Runnable Jar File Path]: Specify the jar file
   * Note:  
   * Example: /spades-data/SpadesAnalytics.jar
* [-a]: Command for class path input
   * Note:  should be “-a”
   * Example: -a
   * [Class Path for Algorithm in Jar]: Specify the the class path for algorithm to run in jar
   * Note:
   * Example: com.qmedic.spades.task.examples.mapreduce.ActivityCount.ActivityCountClean
* [-i]: Command for data input path
   * Note:  should be “-a”
   * Example: -a
* [Data Input Path]: Specify the input path for data
   * Note:  
   * Example: spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*

Input
-----
In our framework, the input format for processing data is restricted to the following, a future work item would be to extend it and allow for a variety of flexible formats…

   * Input Path Folder Format:
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
    * Allowed paths include the following formats: spades-data/development/stanford/2/StanfordStudyYouth2/1/MasterSynced/*

Notes
-----
Here it is assumed that the accelerometer has three axis accelerations
