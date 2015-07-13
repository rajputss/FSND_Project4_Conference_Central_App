## Conference Organization App API Project


## About

This project is not my effort in it's entirety and major thanks to Magnus, Karl and rest of the Udacity staff for making it happen. This is a cloud-based API server that support a web-based and native Android application for conference organization. The API supports the following functionality:

User authentication (via Google accounts)
User profiles
Conference information
Session information
User wishlists
The API is hosted on Google App Engine as application ID full-stack-conference

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Design and Improvement Tasks

1. Add Sessions to a Conference

The following endpoint methods were added:

createSession: given a conference, creates a session.

getConferenceSessions: given a conference, returns all sessions.

getConferenceSessionsByType: given a conference and session type, returns all applicable sessions.

getSessionsBySpeaker: given a speaker, returns all sessions across all conferences.

For the Speaker model design, following datastore properties were implemented:

Property	       |  	Type
--------           ------- 
name		       |	 string, required

highlights      |		string

speaker	       |		string, required

duration	       |		integer

typeOfSession   |		string, repeated

date			    |	   date

startTime	    |		time

organizerUserId |		string

A parent child relationship implementation was used to show one to many sessions. As sessions can be queried by their conference ancestor, it allows for strong consistent querying. 

For speakers, speaker field could be linked to user profiles but that would force speaker to have a registered account which could give inconsistent entry as a result. Session types such as talk, lecture, with session can receive multiple different types.

2- Add Sessions to User Wishlist

The 'wishlist' is accommodated by modifying Profile model where repeated key property field, named, sessionsToAttend are stored. To communicate with the API, two methods were modified to return a unique web-safe key for sessions. 

addSessionToWishlist: given a session websafe key, saves a session to a user's wishlist.

getSessionsInWishlist: return a user's wishlist.

3- Indexes and Queries

Following endpoint methods were added for queries:

-getTBDSessions: returns sessions missing time/date information.

-getConferenceSessionFeed: returns a conference's sorted feed sessions occurring same day or later.

For Specialized Query: There are some limitations when using ndb/Datastore queries since queries are allowed to have on inequality filter only. Firstly, we can query sessions before 7 pm with ndb and then manually filter that list with Python to remove sessions with a workshop type.

4- Add Featured Speaker

createSession endpoint were modified to check whether the speaker appeared in any other conference's sessions. If so, speaker's name and related session name were added to the memcache under featured_speaker key. 
getFeaturedSpeaker was also added as an endpoint to check memcache for the featured speaker.



## Setup Instructions

To deploy this API server locally, ensure that you have downloaded and installed the Google App Engine SDK for Python. Once installed, conduct the following steps:

1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
