## Intro
- "Conference Central" - a cloud-based API server for managing conferences. It
   includes user authentication, user profiles, conference information and
   various manners in which to query the data.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's
   running by visiting your local server's address (by default
   [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.

## Design Decisions
1. Task 1: Sessions are created as children of a conference since they are a
   logical subset of conferences. When I began working on this application I
   imagined the primary use as a simple, easy to use application to be used by
   a large number of people for completely unrelated purposes. As such there
   would be many objects with little reuse. For this reason I chose the fields
   to be simple. The speaker, for example, is a string instead of being modeled
   by a class. If I had imaged the application used by a single firm where the
   same conferences and speakers would be used repeatedly, it would make more
   sense to create a speaker model to maintain data integrity.

   For the endpoints I tried to use both a variety of methods as well as common
   sense for handling the requests. For getting sessions by type I see this as
   best runnning from a form and being posted. This form is easily expanded for
   other filters. But for getting all sessions, and getting sessions by speaker
   it seemed more natural to submit the variables over URL and create GET
   requests.

1. Task 2: Since the wishlist naturally falls under the profile, I chose to add
   a wishlist property to the profile. This was faster and to me more correct
   than implementing an entirely new model. In adhering to my scope of how I
   imagine the application being used, I chose to allow users the freedom
   to add any session to their wishlist. As such I did not constrain them to
   sessions in conferences for which they are already registered. As far as the
   endpoints are concerned, I modeled them after conference registration to
   reduce the amount of new code that had to be written.

1. Task 3:
   Query 1 - Show a list of conferences for which the user has added add
   session to the wishlist but has not registered.
   Query 2 - Find sessions that are schedueld outside of the conerence start
   and end dates.
   Query Problem - The issue with the problem is that indexes cannot be built
   for a multiple inequalities to due the lookup method. To work around this
   limitation I chose to perform one query on each filter, returning only the
   keys, intersecting the common keys, and getting the associated sessions. I
   also thought about querying to get one set of keys, then using those keys
   in an "IN" filter to limit the second query. It's also possible to perform
   one query, fetching all the sessions, and programmatically looping through
   the results to check the other property.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
