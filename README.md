# Canvas_url_parsing
How to create custom Javascript variables in Google Tag Manager to parse Canvas URLs

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

