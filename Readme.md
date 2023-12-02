# Project 2 Report
# Introduction/Background
CineMatch is a web application that aims to provide personalized and group-based movie recommendations. The motivation behind this project is to address the challenges of finding movies that suit the preferences and tastes of individual users and groups of users, as well as to overcome the paradox of choice that arises from having too many options.The project scope is defined by three features: personal recommendations, randomizer, and parties. These cater to individual tastes and group dynamics, while the randomizer adds an element of surprise. The project’s boundaries are set by the TMDB API capabilities, and its limitations include the reliance on user ratings and the accuracy of group recommendations. Our project management approach was chosen from the lessons we learned from working together in Project 1. Tasks were assigned based on individual skills, optimizing workflow and facilitating skill development. GitHub pull requests were used for code review, ensuring high-quality contributions and consistency between the frontend’s calls and the backend’s API. This approach ensured seamless interaction between different parts of the application, and us as teammates.
# Literature Review
While services like IMDB and Letterboxd have made significant strides in the movie industry, they primarily serve as platforms for movie information and social networking, respectively. IMDB, although rich in movie data, is not primarily designed for personalized movie recommendations. On the other hand, Letterboxd, while providing a social platform for movie enthusiasts, lacks a recommendation system altogether. This is where CineMatch diverges from these existing solutions. Unlike streaming services that have proprietary recommendation algorithms, CineMatch is dedicated to providing personalized and group-based movie recommendations, filling a gap in the market. By focusing on this niche, CineMatch aligns with the needs of movie lovers who seek a more tailored movie discovery experience. Thus, CineMatch stands out in its unique approach to movie recommendations.


“A Comprehensive Survey on Movie Recommendation Systems” by Lavanya, R. et al., consolidates and categorizes the major drawbacks present in the most common commercial movie recommendation systems. This is particularly relevant to CineMatch as it seeks to address the above mentioned drawbacks by providing a dedicated platform specifically for movie recommendations. However, CineMatch also is acknowledgedly limited in it’s recommendation algorithm. “Movie Reccomendations Using the Deep Learning Approach” by Lund, J. and Ng, Y. (2018) discusses a deep learning approach for predicting movie ratings. By analyzing a large database of ratings from other users and creating a collaborative filtering system, the system enables personalized movie recommendations. This is a far more advanced and accurate algorithm in comparison to what CineMatch can accomplish. However, this is made up for in terms of direct accessibility to the average person, as previously mentioned.

# Software Technologies
# Frontend
We leveraged the following technologies for our client-side architecture:
- TypeScript as an alternative to JavaScript for improved typing, modules, tooling, etc.
- TMDB API to access movie information to provide recommendations
- React as a frontend library for intuitive design and responsive components
# Backend
We leveraged the following technologies for our server-side architecture:
- JavaScript as our language of choice
- Node.js to provide a runtime environment for our JavaScript server code outside the browser
- Express.js to act a server for our API
- GCP Firebase to persist user and group data like credentials, group members, and film preferences
- Jest to provide a test framework
# Deployment
- Vercel to deploy our website
- GCP App Engine to deploy our server on the web
# Other
- Git/GitHub for version control as our language of choice
- Discord for team communication
- Visual Studio Code for a development environment
# Product Lifecycle
Due to the variety in schedules and outside workload of team members, we our team didn’t perfectly follow a specific methodology. However, because of its agile nature, our team followed our own version of agile.


After project 1, we had a good idea of what kind of tools and requirements we wanted for project 2. This made working separately remotely with large workloads easier. First, we identified our MMFs and technologies to guide development. We then created our repo and left things fairly open ended, stating that any team member who was confident implementing a feature could do so after informing the team. Then, we created skeletons for the front and backend, and connected the backend to GCP Firebase. Once this was done, we contributed small features over time and ran code locally, asking team members for help when needed.


The frontend and backend codebases grew alongside each other, slowly but separately. The API definition was created early so frontend developers could prepare hooks for when the backend was ready. During this time, frontend developers focused on creating views, connecting to TMDB, designing and components, and setting up page routing. The backend devs implemented endpoints, ensuring proper responses, data formatting, and data storage.


Once completed, the backend was deployed to GCP App Engine. Then, frontend devs ran the frontend locally while making calls to the deployed backend to ensure functionality. Once this was verified, the frontend was deployed to Vercel and CineMatch was live. Beyond this point, only small bug fixes were made.
# Requirements
# MMFs
- Movie recommender based on movies rated by user
- A randomizer to select a specific movie from the recommender
- Group movie recommender based on shared accounts
# Functional Requirements
- Users shall be able to create an account with a unique username and password.
- Users shall be able to log in and log out of their accounts.
- Users may have the ability to edit their profiles, including adding a profile picture and updating personal information.
- Users shall be able to browse movies within the application.
- Users shall be able to like and dislike movies.
- Users may be able to leave written reviews for movies they have watched.
- The system shall generate a list of top-rated movies based on a user's likes and dislikes.
- The system should be able to sort movies based on popularity.
- The system should be able to sort movies based on rating.
- Users shall be able to create groups and assign a group name.
- Users shall be able to add others to their groups.
- Group members may be able to see the group’s liked and disliked movies.
- The system shall generate group movie recommendations based on the collective preferences of the group.
- Users shall have the option to use a randomizer to select a movie from their top recommendations.
- The random movie selection should consider the user's preferences and group preferences if applicable.
- Users should be able to view details about the randomly selected movie with a poster.
# Non-Functional Requirements
# Security and Privacy:
- Data transmission between the user's device and the server shall be encrypted using industry-standard protocols (e.g., HTTPS).
- User data shall be securely stored and accessible only to authorized personnel.
- User passwords shall be securely hashed and stored to protect against unauthorized access.
# Availability:
- The system shall have a target availability of at least 99.9% to ensure users can access it reliably.
# Performance:
- The system shall respond to user actions (likes, dislikes, reviews) within 1 seconds.
- The movie recommendation algorithm shall provide results within 5 seconds.
# Scalability:
- The system architecture should be designed to allow for easy scalability to accommodate a growing user base and data volume.
# User-Interface:
- The user interface shall be intuitive and user-friendly, ensuring ease of use for individuals with varying levels of technical proficiency.
- The user interface should have posters of all the movies in the database.
- The user interface may resize according to the users’ type of screen.
# Documentation:
- Comprehensive user and developer documentation shall be provided to facilitate system usage, maintenance, and troubleshooting.
# Design

For our project our team made heavy use of the Pipe and Filter design architecture (see lecture snapshot below). When a user account is created, we do not have any data on the user. However, as the user likes/dislikes movies of certain genres, this information eventually trickles into the database, better enabling our ability to recommend movies that our users will enjoy (more data, better recommendations). This style is very data-centric, which is why we chose the pipe and filter architecture style (it fits like a glove).

![a1.png](pics%2Fa1.png)

Furthermore, the data pipeline is given below:
1) First, the user supplies the CineMatch application with his/her movie preferences in the form of a completed quiz, movie likes/dislikes, etc.


2) Whenever the user wants to find his recommended movies, the application will pull his/her preferences from the database, make some transformations (such as find his/her top 3 most preferred genres using weights), then contact the TMDB API by making an API call.


3) Finally, the recommended movies are returned to the website application. It is important to note that the TMDB database does further filtering of adult content and has its own algorithm of returning movies based on the genres that were provided in the API call, which are of course, not revealed to the API caller (the CineMatch application).



Despite its boons, a flaw of this design approach is that it has too much overhead. By making our filtering step and then calling the TMDB API, which has its own filtering step, then waiting for a response, we are already wasting lots of time performing complex parsing on the data.


Please see the provided pipeline diagram below:

![a2.png](pics%2Fa2.png)


# Testing
- For our testing strategy we divided the tests into whitebox and blackbox testing. Whitebox testing was done in TypeScript using Jest. Given how most of the project was done in typescript, the decision to use Jest made the most sense. Using Jest, we made test cases for individual functions of the backend and TMDB API. First we tested to see if each API was responsive. After that, for the backend API, we tested to see if the app could properly sign up for an account, login with that account, and then delete that account. The created user details were randomized so that if the test is repeated, there will be little chance for error caused by previous executions of the test. For example, if a test fails to delete the user, then this should not cause the create user of the next test to fail. The TMBD API had simpler tests. The tests for the TMDB API were to see if it was responsive and to see if it returned accurate movie data given a specific movie ID to search. All these tests passed when ran individually and ran together.
- Blackbox testing was done manually following a checklist based on the state diagram between the different pages of the app. The list of actions tested manually are create new user, log in, log out, rate, save ratings, create party, add member, call recommendation algorithm, and return result.
- All this testing helped with the development of the app. Errors were easy to identify. For example, when the sign up functionality wasn’t working, we knew it was something to do with the backend because it failed both in whitebox testing and blackbox testing. If it had passed whitebox testing, then we would know that there was some error in the frontend code. We later found out that the backend sign up was set to ‘GET’ instead of ‘POST’. The easier diagnosis of problems led to a smoother development process overall.
