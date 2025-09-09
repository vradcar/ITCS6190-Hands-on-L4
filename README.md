# Project Overview
This project implements a Hadoop MapReduce job in Java to perform word count on a text file. It counts the occurrences of each word with 3 or more characters and outputs the results in descending order of frequency.

# Approach and Implementation
- **Mapper:** Reads each line, splits it into words, and emits (word, 1) for each word with 3 or more characters.
- **Reducer:** Sums the counts for each word and outputs (word, total count).
- **Sorting:** The output is sorted in descending order of frequency after the reduce phase.

# Execution Steps
1. Build the project using Maven.
2. Run the Hadoop MapReduce job using Docker.
3. The input file is located at `shared-folder/input/data/input.txt`.
4. The output file is generated at `shared-folder/output/part-r-00000`.

# Challenges Faced & Solutions
- Having had Maven and Java previously set up, I did not have to do any separate setup. I just ran the entire program according to the instructions in the readme.md file after creating an input text. It ran through without any problems executing as expected.

# Input and Obtained Output

**Input Example:**
```
My name is Yoshikage Kira. I'm 33 years old. My house is in the northeast section of Morioh, where all the villas are, and I am not married. I work as an employee for the Kame Yu department stores, and I get home every day by 8 PM at the latest. I don't smoke, but I occasionally drink. I'm in bed by 11 PM, and make sure I get eight hours of sleep, no matter what. After having a glass of warm milk and doing about twenty minutes of stretches before going to bed, I usually have no problems sleeping until morning. Just like a baby, I wake up without any fatigue or stress in the morning. I was told there were no issues at my last check-up. I'm trying to explain that I'm a person who wishes to live a very quiet life. I take care not to trouble myself with any enemies, like winning and losing, that would cause me to lose sleep at night. That is how I deal with society, and I know that is what brings me happiness. Although, if I were to fight I wouldn't lose to anyone.
```

**Output Example:**
```
and	6
the	5
I'm	4
that	3
with	2
morning.	2
not	2
were	2
any	2
like	2
get	2
lose	2
what	1
without	1
all	1
would	1
Yoshikage	1
sleeping	1
stores,	1
hours	1
going	1
explain	1
fatigue	1
stretches	1
northeast	1
about	1
usually	1
villas	1
house	1
who	1
before	1
wishes	1
After	1
don't	1
for	1
old.	1
fight	1
live	1
make	1
PM,	1
sleep,	1
check-up.	1
winning	1
Morioh,	1
Kame	1
warm	1
Kira.	1
sleep	1
was	1
losing,	1
there	1
know	1
take	1
name	1
stress	1
enemies,	1
last	1
married.	1
eight	1
cause	1
home	1
person	1
wake	1
department	1
issues	1
occasionally	1
bed,	1
matter	1
doing	1
latest.	1
That	1
Although,	1
glass	1
life.	1
smoke,	1
having	1
section	1
society,	1
until	1
twenty	1
wouldn't	1
how	1
care	1
dataset	1
Just	1
every	1
night.	1
employee	1
deal	1
sure	1
drink.	1
have	1
work	1
input	1
trouble	1
milk	1
trying	1
bed	1
but	1
are,	1
minutes	1
problems	1
own	1
Create	1
quiet	1
told	1
baby,	1
very	1
your	1
where	1
happiness.	1
years	1
day	1
brings	1
what.	1
anyone.	1
myself	1
```


# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset. In this project we provide a input file and then we create a maaper and reducer logic to count the occurence of each word in the given input. There are sample input and Expected output for the sample input.

## Approach and implementation
1. Mapper Logic: We use StringTokenizer to create tokens from the input file and loop it using while loop to map all the words in the input file with key value pairs. In this mapper, it will not count characters that are smaller than 3.

2. Reducer Logic: Using the output of Mapper logic we increase a variable sum value as we encounter same words and retun them. this way we will get a list of words and the number of times it occured in the input file as output.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/data
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/data
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output


## Sample Input: 
 ```bash
   Hello world
   Hello Hadoop
   Hadoop is powerful
   Hadoop is used for big data
   ```

## Expected output: 
 ```bash
Hadoop 3
Hello 2
used 1
for 1
big 1
data 1
powerful 1
world 1
   ```
