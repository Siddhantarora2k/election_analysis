# election_analysis

## Project overview
The purpose of this challenge was to determine the _winner_ county from the **Election**.

## Findings
###### 1. Total number of votes received.
###### 2. Total number of votes each **Candidate** received.
###### 3. Total turnout of largest **County**.
###### 4.a) Which county turnout was the largest?
######  b) By how much?
######  c) By how much percentile?
###### 5.a) Which candidate received the highest number of votes?
######   b) Every candidate's votes.
######   c) Candidate's winning percentage.


First we add our libraries

    # Add our dependencies.
    import csv
    import os

Then, we need a variable to _load_ and _save_ our file.

          # Add a variable to load a file from a path.
          file_to_load = os.path.join("Resources/election_results.csv")
          # Add a variable to save the file to a path.
          file_to_save = os.path.join("analysis", "election_results.txt")


To count the total votes we need to set it to zero initially.

      # Initialize a total vote counter.
      total_votes = 0


Then we create a _list_ of Candidate options, and a _dictionary_ of each candidates votes.

      # Candidate Options and candidate votes.
      candidate_options = []
      candidate_votes = {}

Create a county list and county votes dictionary.

    county_options = []
    county_votes = {}

Create a string variable for winning candidate and integer variables for winning count and winning percentage. 
           
           # Track the winning candidate, vote count and percentage
            winning_candidate = ""
            winning_count = 0
            winning_percentage = 0


Track the largest county and county voter turnout.
      
        largest_county_turnout = ""
        largest_county_number_turnout = 0


Read the csv and convert it into a list of dictionaries
   
        with open(file_to_load) as election_data:
        reader = csv.reader(election_data)
        
Reading the header

    # Read the header
    header = next(reader)

Creating a loop for each row in the CSV file.

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

Creating a conditional statement into the loop which will eventually add every name and votes by 1 through each row.

        # If the candidate does not match any existing candidate add it to the candidate         # list
        
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

Similarly, Writing a conditional statement for the county.

        # 4a: Write a decision statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_options:

            # 4b: Add the existing county to the list of counties.
            county_options.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1

_Printing_ the Results and _Saving_ the results to the **text file**

    # Save the results to our text file.
    with open(file_to_save, "w") as txt_file:

        # Print the final vote count (to terminal)
        election_results = (
            f"\nElection Results\n"
            f"-------------------------\n"
            f"Total Votes: {total_votes:,}\n"
            f"-------------------------\n\n"
            f"County Votes:\n")
        print(election_results, end="")

        txt_file.write(election_results)

Creating a loop for the county.

    # 6a: Write a repetition statement to get the county from the county dictionary.
    for county_name in county_votes:
        # 6b: Retrieve the county vote count.
        county_vote_count = county_votes[county_name]
        # 6c: Calculate the percent of total votes for the county.
        county_vote_percentage = float(county_vote_count) / float(total_votes) * 100

Printing the county votes and percentage.

         # 6d: Print the county results to the terminal.
        county_results = (f"{county_name}: {county_vote_percentage:.1f}% ({county_vote_count:,})\n")
        print(county_results)
         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
        
Writing a conditional statement to determine the winning county.

         # 6f: Write a decision statement to determine the winning county and get its           vote count.
         
        if (county_vote_count > winning_count):
            #If true:
            winning_count = county_vote_count
            largest_county_turnout = county_name
Printing the Largest county and the its total turnout.

    # 7: Print the county with the largest turnout to the terminal.
    largest_county_summary = (
        f"----------------------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}: {winning_count}\n"
        f"----------------------------------------\n")
    print(largest_county_summary)
    
Finally, Saving the results to the text file.

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_summary)
    
    
    
Save the final candidate vote count to the text file.

    winning_count = 0
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
    
# Conclusion
1. Denver was the largest county with a turnout of **306055** comprising of over **82%** of the total turnout.
2. Diana DeGette was the winning candidate with **272,892** comprising of more than **73 %** of total votes.
