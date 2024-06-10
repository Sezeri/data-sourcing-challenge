# data-sourcing-challenge
This challenge is for Module 6. It was, by far, the most time consuming of all the challenges I have completed so far; not necessarily the most challenging. 
I must note that I learned a lot more with this challenge than I did with the project 1.
One must note  a minor insufficiency in the code as a direct result from an insufficiency in the instructions. 
To illustrate this infficiency, the code below was my initial code where any and all multiples of 50 would be processed to resolve the insufficiency. My designed scriot even has a control script to reject invalid multiplication factors. But I strugled to get out of a loop (the one that validates the user's input where it only runs one time and find a movie title, then return to the top and asks for another multiplicator) and did not get cooperation from any TA to correct this. Some suggested I do without that user input and validation; which I ended up doing. The only get-around from that insufficiency is the script I am submitting, which also meets all the requirements of the challenge, including that minor inssufficiency. I am hoping to get assistance from a TA or my tutor to address this insufficiency so I can be happy with the code.

Below is the script I had wanted initially, but could not figure out how to get out of the loop it is stuck in.

# Create an empty list to store the results
tmdb_movies_list = []
# Create a request counter to sleep the requests after a multiple
# of 50 requests
request_counter = 1
# Loop through the titles
for title in titles:
    # This is to validate the user's input and ensure it is a valid multiple factor
     # Simulating multiple of 50 requests
     m = input("Hello there! What multiple of 50 would you like to process? ")
     # This is to turn the user's input into an integer
     m = int(m)
#     # Check if the input is valid
     if m < 1:
         print("You must enter a multiple factor greater than or equal to 1. Please try again.")
         # Restart the loop to get a valid input
         continue
     if m >=1:
    # Check if we need to sleep before making a request
        if request_counter % 50 == 0:
            time.sleep(1)
            print(f"Sleeping at {request_counter} requests")
        else:
    # Update counter
            request_counter += 1
    # Perform a "GET" request for The Movie Database
        response = requests.get(url + title + tmdb_key_string)
        data = response.json()
    # Include a try clause to search for the full movie details.
    # Use the except clause to print out a statement if a movie
    # is not found.
        try:
            # Get movie id
            movie_id = data["results"][0]["id"]
            # Make a request for a the full movie details
            query_url = f"https://api.themoviedb.org/3/movie/{movie_id}?api_key={tmdb_api_key}"
            # Execute "GET" request with url
            data = requests.get(query_url).json()
            # Extract the genre names into a list
            genres = []
            for genre in data['genres']:
                genres.append(genre["name"])
            # Extract the spoken_languages' English name into a list
                spoken_languages = []
            for language in data['spoken_languages']:
                spoken_languages.append(language["english_name"])
        # Extract the production_countries' name into a list
            production_countries = []
            for country in data['production_countries']:
                production_countries.append(country["name"])
            # Add the relevant data to a dictionary and
            # append it to the tmdb_movies_list list
            tmdb_movies_list.append(
            {
                "title": data['title'],
                "original_title": data['original_title'],
                "budget": data['budget'],
                "genre": genres,
                "language": data['original_language'],
                "spoken_languages": spoken_languages,
                "homepage": data['homepage'],
                "overview": data['overview'],
                "popularity": data['popularity'],
                "runtime": data['runtime'],
                "revenue": data['revenue'],
                "release_date": data['release_date'],
                "vote_average": data['vote_average'],
                "vote_count": data['vote_count'],
                "production_countries": production_countries
            }
                                    )
            # Print out the title that was found
            print(f"Found {title}")
        except:
            print(title + " not found.")
        if request_counter % 50 == 0:
            time.sleep(1)
            print(f"Sleeping at {request_counter} requests")
            break
    
# Preview the first 5 results in JSON format
# Use json.dumps with argument indent=4 to format data
#json_results = json.dumps(results[:5], indent=4)
# Print the result
#print(json_results)
