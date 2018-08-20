Send Canvas Course ID and Activity ID to Google Analytics
---

How to create custom Javascript variables in Google Tag Manager to parse Canvas URLs to send to Google Analytics. 

Our institution recently enabled Google Analytics on the institution's Canvas installation. For the most part we followed the steps provided by [Instructure](https://community.canvaslms.com/docs/DOC-9211-how-to-set-up-google-analytics-for-canvas). 

I started to run QA in GA and it quickly became clear that the variables were not firing on all pages they should have been. The Canvas URL mapping is quite straightfoward, and takes this form:

* Institution's main page: INSTITUTION.instructure.com/
* Any course: INSTITUTION.instructure.com/courses
* A specific course: INSTITUTION.instructure.com/courses/2345678/
* A courses' assignments page: INSTITUTION.instructure.com/courses/2345678/assignments
* A courses' assignment: INSTITUTION.instructure.com/courses/2345678/assignments/234532

The Course ID variable was being triggered, however, it was not being triggered on sub-pages. For instance, a visit to `INSTITUTION.instructure.com/courses/2345678/` triggered a pageview, but not `INSTITUTION.instructure.com/courses/2345678/assignments`. This is clearly problematic if one is interested in analyzing course engagement. 

Instead of trying to debug the possible things Canvas could send to GA, we decided to parse the URL to send the relevant information to GA. 

I worked with our GA deleloper to use Google Tag Manager and created custom variables that are triggered on all pageviews. These variables parse the URL into portions we are interested in recording. 

I began by writing a regular expression that would pull out the course ID:
`(?:/courses/)([0-9]+)` (*note:* sometimes backslashes need to be escaped with "\\")
If you are interested in learning how regular expressions work, I recommend playing around with <https://regex101.com>. 

The second part was figuring out how to get Tag Manager to send the number as an output to GA. I found the easiest way to do this was to write a custom Javascript function saved into a new variable. 

`function() {
   var re = /(?:\/courses\/)([0-9]+)/;
   var result = re.exec({{Page Path}})
   return result[1]
}`

You can see based on the above URL's the a natural next extension is to send the activity ID as well. We obtained an activity variable script from a neighboring institution which works quite well. It essentially captures the word after the course ID in the URL and sends it as the Activity (e.g., Assignment, Quiz, Discussion). 

To get the Activity ID the above function needs only be extended by adding non-capturing groups: 

`function() {
    var re = /(?:\/courses\/)(?:[0-9]+\/)(?:[a-zA-Z]+\/)([0-9]+)/;
    var result = re.exec({{Page Path}})
    return result[1]
}
`
