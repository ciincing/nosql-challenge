# nosql-challenge

## Instructions
The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. You've been contracted by the editors of a food magazine, Eat Safe, Love, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.

## Part 1: Database and Jupyter Notebook Set Up

I began by setting up the database and Jupyter Notebook for the task, using the provided NoSQL_setup_starter.ipynb file.
First, I imported the data from the establishments.json file into MongoDB using the Terminal. I named the database "uk_food" and the collection "establishments." The following is the command I used for this:
  [mongoimport --db uk_food --collection establishments --file establishments.json]
After successfully importing the data, I proceeded to the Jupyter Notebook.
Within the Jupyter Notebook, I imported the necessary libraries: PyMongo and Pretty Print (pprint). This was done as follows:
  [from pymongo import MongoClient
  from pprint import pprint]
Next, I created an instance of the Mongo Client: [client = MongoClient()]
To confirm that the database and data were loaded properly, I executed the following steps:
1. Listed the databases in MongoDB and verified that "uk_food" was listed
2. Listed the collections in the "uk_food" database to ensure that "establishments" is present
3. Found and displayed one document in the "establishments" collection using the find_one method and displayed it using pprint
4. Finally, I assigned the "establishments" collection to a variable to prepare it for further use
With these steps completed, I had successfully set up the database and Jupyter Notebook for the task and confirmed the proper loading of data for subsequent analysis.

## Part 2: Update the Database

The magazine editors had some requested modifications for the database before I could perform any queries or analysis for them. I made the following changes to the establishments collection.
An exciting new halal restaurant had just opened in Greenwich, but it hadn't been rated yet. The magazine asked me to include it in the analysis. I added the following information to the database:
  [{
    "BusinessName":"Penang Flavours",
    "BusinessType":"Restaurant/Cafe/Canteen",
    "BusinessTypeID":"",
    "AddressLine1":"Penang Flavours",
    "AddressLine2":"146A Plumstead Rd",
    "AddressLine3":"London",
    "AddressLine4":"",
    "PostCode":"SE18 7DY",
    "Phone":"",
    "LocalAuthorityCode":"511",
    "LocalAuthorityName":"Greenwich",
    "LocalAuthorityWebSite":"http://www.royalgreenwich.gov.uk",
    "LocalAuthorityEmailAddress":"health@royalgreenwich.gov.uk",
    "scores":{
        "Hygiene":"",
        "Structural":"",
        "ConfidenceInManagement":""
    },
    "SchemeType":"FHRS",
    "geocode":{
        "longitude":"0.08384000",
        "latitude":"51.49014200"
    },
    "RightToReply":"",
    "Distance":4623.9723280747176,
    "NewRatingPending":True
}]

I proceeded to complete the following tasks as per the magazine editor's request in NoSQL_setup_starter.ipynb:
1. First, I needed to find the BusinessTypeID for "Restaurant/Cafe/Canteen" and return only the BusinessTypeID and BusinessType fields. Here's how I did it
2. I then updated the information for the new restaurant that was added earlier with the BusinessTypeID I found.
3. The next task was to identify and remove all establishments located in Dover Local Authority. I started by counting the number of documents with Dover Local Authority.
4. To remove these establishments, I used the following command: [establishments.delete_many({'LocalAuthorityName': 'Dover'})]
5. After deletion, I checked the number of documents to ensure that the establishments in Dover were removed.
6. To address the issue of number values being stored as strings, I used the update_many method to convert latitude, longitude, and RatingValue to the appropriate data type
   - Use update_many to convert latitude and longitude to decimal numbers
   - Use update_many to convert RatingValue to integer numbers


## Part 3: Exploratory Analysis

In NoSQL_analysis_starter.ipynb, I had specific questions from Eat Safe, Love that I needed to answer to help them find the locations they wished to visit and avoid. I took note of a few important points while exploring the dataset:

RatingValue is the overall rating, ranging from 1 to 5, with higher values indicating better ratings. It may also include non-numeric values like 'Pass,' which I needed to handle.

The scores for Hygiene, Structural, and ConfidenceInManagement work in reverse, where higher values indicate worse performance.

For each question, I followed a structured approach:

Used count_documents to determine the number of documents contained in the result.
Displayed the first document in the results using pprint.
Converted the result to a Pandas DataFrame, printed the number of rows in the DataFrame, and displayed the first 10 rows.
Here are the answers to the questions:

Question 1: Which establishments have a hygiene score equal to 20?
To find this, I used the following code: [query = {'scores.Hygiene': 20}]

Question 2: Which establishments in London have a RatingValue greater than or equal to 4?
I used a regex to find London Local Authorities and then filtered establishments with a RatingValue greater than or equal to 4. 

Question 3: What are the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?
To answer this question, I compared the geocode to find the nearest locations. I searched within 0.01 degrees on either side of the latitude and longitude.

Question 4: How many establishments in each Local Authority area have a hygiene score of 0? Sort the results from highest to lowest, and print out the top ten local authority areas.
To answer this question, I used the aggregation method. Here is what the first five rows were supposed to look like: 
<img width="586" alt="image" src="https://github.com/ciincing/nosql-challenge/assets/130705911/63ca732f-70b7-4ec0-8ace-af94fd9b84a3">
