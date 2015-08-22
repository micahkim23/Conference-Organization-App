## About

App Engine application for the Udacity training course. This project is a cloud-based API server to support a web-based application for conference organization. The API supports:
1. User authentication (via Google accounts)
2. User Profiles
3. Conference information
4. Session information
5. User Wishlists

The API is hosted on Google App Engine as application ID conference-app-1027, and can be accessed through the API Explorer.

## Design Tasks

###TASK 1: Add Sessions to a Conference

I added the following endpoint methods:

1. `createSession: given a conference, creates a session`
2. `getConferenceSessions: given a conference, returns all sessions`
3. `getConferenceSessionsByType: given a conference and session type, returns applicable sessions`
4. `getSessionsBySpeaker: given a speaker, returns all sessions among all conferences`

For the Session design, I implemented the following datastore properties:

|Property  		| Type 				|
|---------------|-------------------|
|name 	  		| string, required	|
|highligts	 	| string			|
|speaker 	 	| string, required	|
|duration 	 	| integer			|
|typeOfSession	| string, repeated	|
|date 			| date 				|
|startTime 		| time 				|
|organizerUserID| string 			|

A parent-child implementation was used to represent the conference to many sessions relationship. Sessions can be queried by their conference ancestor. 

Speakers were included as part of the Session model, so that they wouldn't need to have an account to be registered. This is why I didn't link the speaker field to user profiles.

Sessions types were implemented in a "tag" representation, so that they would be able to be identified by multiple types.

###TASK 2: Add Sessions to User Wishlist

I modified the Profile model to store a wishlist with a repeated key property field, sessionKeysToAttend. This stores the web-safe keys for sessions that user would like to attend. I added the following two endpoint methods to the API:

1. `addSessionToWishlist: given a session websafe key, saves a session to a user's wishlist.`
2. `getSessionsInWishlist: return a user's wishlist`

###TASK 3: Indexes and Queries

I added two endpoint methods for addition queries that might be useful for this application:

1. `getSessionsByCity: given a city, returns all sessions for that city`
2. `getSessionsTBD: returns all sessions that are missing any date/time information such as date, duration, or startTime`

For the query related problem that queries for all non-workshop sessions before 7pm, the problem is that an inequality filter can be applied to at most one property. You cannot query for sessions that are not workshops, while at the same time query for sessions that occur before 7pm. One solution is to run a query for sessions that are not workshops and also run a different query for sessions before 7pm. Then, manually filter out the results for the overlap region that includes non-workshop sessions before 7pm.


###TASK 4: Add Featured Speaker

I modified the createSession endpoint to check and see if the speaker was in any other of the conferenece's sessions when creating a new session. If so, the newly added session's speaker would become the next featured speaker. The featured speaker would be added to the memcache under the MEMCACHE_SPEAKERS_KEY. I also added the following endpoint:

1. `getFeaturedSpeaker: check memcache for the featured speaker`

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions

Make sure you have Google App Engine SDK for Python installed before conducting the following steps.

1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
2. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
3. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
4. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
5. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
6. (Optional) Generate your client library(ies) with [the endpoints tool][6].
7. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
