# PBIs

- May 1 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    As developers we need to know which framework to use so that are contributions are compatible.
    
    As a front end developer I need to know the routing and layout from initial page to (dashboard/default page?) and other pages of the app so that I can visualize it and implement it.
    
    As a front end developer I need a CSS framework (set of variables, classes, etc) to use in development to create a consistent site experience.
    
    As a user I want to see a header bar with the following buttons: sign-up, log in, (log out - if logged in), theme (sun, moon), and site pages, so that I can easily navigate the app.
    
    As a front end developer I need the routing and page templates created so that I can add components to the app.
    
    As a developer I need to know the common design principals (shape, styling, look and feel) for creating cards/containers for data and other common components to to ensure consistency in design and less duplication in the CSS.
    
    As a front end developer I need to provide implementation for Signing Up through a menu with restrictions on what the user can use as a password so the user can use the site.
    
    As a front end developer Logging In, and Logging Out through menus and forms so that the user can access their data and use the rest of the website.
    
    As a front end developer I need a way to let the user know when an error has occurred so that the user can know what to do next.
    
    - Eg: If the user types in the wrong password, it will say incorrect password and they will know to try again.
        - email already taken
        - invalid login credentials
        - etc.
    - Could be done like a notification: slides down from the top of the screen and then disappears after a set amount of time without blocking the use of the website
    
    ---
    
    <aside>
    💡 Back End
    
    </aside>
    
    - As a back end developer I need to maintain server-side endpoint documentation so the front end developers know how to request and access the server.
    - As back end developers we need to decide what kind of database to use so we can start being able to store user data when they create an account.  Potential options: SQL, MongoDB, etc.
    - As a back end developer I need to provide implementation for a user to be able to Sign Up (create an account with validation for passwords)
    - As a back end developer I need to Log In a user and provide them with an access token, so that future requests can be authenticated.
    - As a back end developer I need to Log Out a user and invalidate the access token, so that someone can’t access the data without signing back in.
- May 15 Sprint
    - (Planning/Notes)
        
        [Front End]
        
        - Need to add login/signup/logout to header bar on main page
        - Make a main page/dashboard
            - you get taken here after you login or signup
            - this will have the same header bar as the home page
            - Additional content that needs to be displayed here: (these can be pages or components/popup windows)
                - view profile + button to switch to an editable profile page
                - create contest
                - list/search contests
                - view invitations (maybe as an icon in the header and it brings down a list with your notifications, so you can quickly see without going to another page)
                - view pending missions
                - (these things also could be displayed in a tile like format on the main/dashboard page
        - View profile page + edit profile
        - Create contest page/component (maybe better as a large popup window the people can hit cancel and go right back to the page they were on)
        
        ---
        
        [Back End]
        
        - Get profile information
        - Overwrite/change profile information
        - Create contest
        
        If need more…
        
        - Get user’s notifications
        - Accept/Reject mission invitation
        
    
    ---
    
    <aside>
    💡 [Front End]
    
    </aside>
    
    As a front-end developer I need to make a profile page so that users can see information such as their first and last name or their email and be able to edit this information if they would like.
    
    As a front-end developer I need to create a “create/host contest” page so that contests can be created and at a later point have other users join the contest.
    
    As a front-end developer I need to create a component to view a contest’s details so that the user knows what that particular contest is about or check prize information and view the following information:
    
    - scoreboard
    - contest details
    - available missions
    - participants
    
    As a front-end developer I need to create a page/component to search for different contests based on a set of criteria.
    
    (More Potential PBIs)
    
    As a front-end developer I need to create a display for notifications a user would have so that the user can know when they are invited to a contest and be able to accept/reject the invitation. (Just a display, no real functionally yet)
    
    ---
    
    <aside>
    💡 [Backend]
    
    </aside>
    
    As a back-end developer I create a way for the front-end developers to request and retrieve their profile information so that the user can view this information.  
    
    **Done.** 
    
    - **Users can retrieve their current profile using GET /user/profile**
    
    As a back-end developer I create a way for the front-end developers to overwrite/change their profile information so that the user can change this information if they choose. 
    
    **Done.**
    
    - **Users can modify their profile using PATCH /user/profile**
    
    As a back-end developer I need to make a way for the front-end to request creating a contest so that other users can eventually join it.  
    
    **Done.**
    
    - **Users can create a contest using POST /contest**
    
    As a back-end developer I need to make a way for the front-end to request the information/details of a contest and extra information or properties if you are the host of the contest so that users can view this information.  ****
    
    **Done.** 
    
    - **Users can get a contest that is public or they’re hosting, invited to, participating in, or judging using GET /contest/{id}**
    - **Hosts can get all of the contests they’re hosting using GET /contests/hosted**
    - **All users can get all public contests using GET /contests**
    
    As a back-end developer I need to make a way for the front-end to search created contests based on criteria and retrieve a list back so that users will be able to join open public contests they weren’t specifically invited to. 
    
    **Done.**
    
    - **Get /contests**
    
    As a back-end developer I need to make a way for a user to request joining a contest (automatically lets them in if the contest is public open and there is no restriction) so that users are able to request joining a contest. 
    
    **Done.** 
    
    - All private contests are by invitation only.  Public contests can be open to all or require users to submit requests.
    - **Users can join or leave an open public contest by using PATCH /contest/{id}/membership?action=join and PATCH /contest/{id}/membership?action=leave, respectively.**
    - **Users can request to join a public contest that requires approval using PATCH /contest/{id}/membership-request?action=request.**
    - **Hosts can approve membership requests using PATCH /contest/{id}/membership-request?action=evaluate.**
    - **Notification is added to user’s notification array if membership is approved by host.**
    - **Notification is sent to host when user requests membership**
    
    ////////////////////////// [ More Backend ] (5/17/23)
    
    As a backend dev I need to make a way to invite another user by email address to join a contest (invitations go to their notifications list) so users can be added to private contests. 
    
    **Partially done.**
    
    - **A host can invite set of people (using their emails) to be participants in a contest using POST /contest/{id}/invitation.**
    - **If the email address is for a registered user a notification is added to the user’s notification array.**
    - Need to send emails to non-users.
    - Need to add preferences to user profile to allow sending of notification and/or sending of email.
    
    As a backend dev I need to make a way for a user to [Accept/Reject] an invitation so that the user can get into a private contest. If they accept they have successfully joined the contest. If they reject they aren’t.  
    
    **Done**. 
    
    - **Users can accept or reject the invitations using PATCH /contest/{id}/invitation?role=participant&action=accept and PATCH /contest/{id}/invitation?role=participant&action=reject, respectively.**
    
    As a backend dev I need to make a way for a host to invite specifically a judge to a contest and have [Approve/Deny] where if they accept they go into a different list of specifically judges so that there will be people in the contest who will judge the submissions.   
    
    **Done.** 
    
    - **A host can invite set of people (using their emails) to be judges in a contest using POST /contest/{id}/invitation.**
    - **Users can accept or reject the invitations using PATCH /contest/{id}/invitation?role=judge?action=accept and PATCH /contest/{id}/invitation?role=judge&action=reject, respectively.**
    
    As a backend dev I need to make a way to edit the contest details if the user is the host (description, prize, remove participant, etc.) so that the info of a contest can be changed and fixed.  
    
    **Done.**
    
    - **A host can modify a contest that they created using PATCH /contest/{id}**
    
    As a backend dev I need to make a way to create a mission within a contest and edit the properties so that there will be missions in the contest.  
    
    **Done.** 
    
    - A h**ost can create a mission using** **POST /contest/{id}/mission**
    - **A host can modify a mission using PATCH /contest/{id}/mission/{id}**
    
    As a backend dev I need to make a way to view all the missions within a contest so that the users can know what they can do to earn points. (is this already included in getting the contest info?).
    
    **Done.**
    
    - **Missions are included in all routes that retrieve contest information,  i.e. GET /contest/{id}, GET /contests and GET /contests/hosted**
    
    **Notifications**
    
    As a backend dev I need to make a way to retrieve a users notifications so they can know when they’ve been invited to contests, etc.
    
    - **Users can view their notifications using** **GET /user/notifications**
    - **Users can delete one notification using DELETE /user/notifications?id={id} and delete all notifications using DELETE /user/notifications?all=true**
    
    Is there a way to send a notification to someone on the event that someone invites them? because the user may not check the app that often and they wouldn’t see the invite for a while. **EM: I think we can use the [Push API](https://www.w3.org/TR/push-api/).**
    
    Would sending an email to user’s connected account be okay, as long as we obtain permission to send communications to their email on registration? -paul. **EM: Hi, Paul.  Yes. We can use [sendgrid](https://sendgrid.com) to send emails.**
    
- May 29 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    As a front-end developer I need to allow users to check their notifications so they can know when they’ve been invited to a contest.
    
    As a front-end developer I need to allow users to view a list of all the contests they have created so they can know this information.
    
    As a front-end developer I need to allow users to view the list of contests they are currently a part of and be able to leave a particular contest so the user can leave or see which ones they are currently in.
    
    As a front-end developer I need to allow users to join/request to join a particular contest so they can join it.
    
    As a front-end developer I need to allow users to accept an invitation to join a private contest.
    
    As a front-end developer I need to allow contest hosts to view a list of pending requests so they can accept or deny them.
    
    ---
    
    <aside>
    💡 Back End
    
    </aside>
    
    As a back-end developer I need to allow users to retrieve a list containing the participants (and their scores) in the contest so they can view that information.
    
    - **Done.** All contestants of a contest and their scores are returned by GET /contest/{id}
    
    As a back-end developer I need to allow users to delete a contest they’ve made in case they want to.
    
    - **Done.** DELETE /contest/{id}
    
    As a back-end developer I need to allow contest hosts to delete a mission if they choose.
    
    - **Done.** DELETE /contest/{id}/mission/{id}
    
    As a back-end developer I need to allow users to get their current total score with the completed missions within a particular contest so they can see where they are at.
    
    - **Done.** GET /contest/{id} returns arrays named pendingMissions, acceptedMissions, and rejectedMissions holding submissions that have not been adjudicated yet, have been approved by a judge, and have been rejected by a judge respectively.
    
    As a back-end developer I need to add the notification type to sent notifications so the user can know what type the notification is.
    
    Types:  
    
    - ContestantInvitation
    - JudgeInvitation
    - ParticipationRequestReceived
    - ParticipationRequestApproved
    
    **Done**.  notificationType property added to all notifications.
    
    As a back-end developer I need to allow the user to submit data for a particular mission (complete a mission) so that the user can submit their work and have it reviewed later.
    
    Types:  [”TextSubmit”]
    
    - **Done.** Users can submit missions to be adjudicate using POST /contest/id/mission/id/submission.
    
    **Not complete:** As a back-end developer I need to allow searching for contests by name for users to find them more easily.
    
- June 12 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    As a front-end developer I need to allow the host of a contest to create missions for that contest.
    
    As a front-end developer I need to allow the user to view all the missions that are in a contest.
    
    As a front-end developer I need to allow the host of a contest to delete the contest if they want.
    
    As a front-end developer I need to allow the host of a contest to delete a mission if they want.
    
    As a front-end developer I need to allow the user to check their notifications in a list so that if some have required input (such as being invited to a contest) the user will be able to accept or reject that.
    
    As a front-end developer I need to allow the user to check what their current score is in a contest.
    
    As a front-end developer I need to allow a user to submit text for a mission. (with dropdown to select judge to judge that mission)
    
    ---
    
    <aside>
    💡 Back End
    
    </aside>
    
    (Bug) Leaving contest that requires approval doesn’t work.
    
    - **Fixed**
    
    As a back-end developer I need to allow searching for contests by name for users to find them more easily.
    
    - **Not started - we slacked off this sprint!**
    
    As a back-end developer I need to fix adjudication to be an array to allow multiple in one request, and allow accepting and rejecting in the request.
    
    - **Done.** PATCH /contest/:cid/mission/:mid/adjudication takes array of *******cases******* in the request’s body. Each case requires a `sid` (submission id) property and a `status` property (’Approved’ or ‘Rejected’) and can include an optional `justification` property.
    
    As a back-end developer I need to have the user’s First Name, Last Name, and Email in the list of contest join requests, so that the contest host knows who they are accepting or rejecting. 
    
    - **Done**. GET /contest/:id returns participationRequests[] which contains objects that include user’s first name, last name, and email.
    
    As a back-end developer I need to allow a participant to submit a file attachment as a submission to a contest.
    
    - **Done**.  POST /contest/:cid/mission/:mid/submission allows contestants to submit TextUpload and SingleFileUpload submissions.
    
    As a back-end developer I need to allow the user to view the data they’ve submitted to a mission.
    
    - **Done**: GET /contest/:cid/mission/:mid/submission/:sid returns all of the data for a submission to the user who submitted the submission or to the judge assigned to the submission.
    
    As a back-end developer I need to allow the user to withdraw their submission to a mission.
    
    - **Done**: DELETE /contest/:cid/mission/:mid/submission/:sid deletes a submission.
    
    As a back-end developer I need to allow the user to edit a submission for a mission.
    
    - **Done**: PATCH /contest/:cid/mission/”mid/submission/:sid/validationMethod allows the user who submitted the submission to modify the judge, text, or file in the submission.   If current status is ‘Rejected’ and user has not exceeded max number of submissions, status is changed to ‘Pending’.
    
    **Question**: What data should be deleted when a user leaves a contest?  completed missions?
    
- June 26 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    Change the header have it’s data filled in from JavaScript so there isn’t inconsistency between the pages that use a header.
    
    Fix up the Login/Signup Pages
    
    - to work with new design and colors
    - remove debugging alerts
    
    View Contest Leaderboard
    
    Delete a Mission
    
    Submit data to be approved for a Mission (TextUpload, SingleFileUpload)
    
    Fix mission modal
    
    - to make it closable by clicking outside of the box
    - not show the submit button if you aren’t current enrolled in the contest
    
    Allow contest hosts to invite specific people to a contest
    
    Allow contest hosts to invite judges to a contest/mission
    
    Label if you are a Contestant, Host, or Judge when viewing a particular contest with how many points you have if you are a Contestant
    
    ---
    
    <aside>
    💡 Backend
    
    </aside>
    
    Allow Searching Contest By Name
    
    **Done!**
    
    - Add index to Contest schema ([https://stackoverflow.com/questions/28775051/best-way-to-perform-a-full-text-search-in-mongodb-and-mongoose](https://stackoverflow.com/questions/28775051/best-way-to-perform-a-full-text-search-in-mongodb-and-mongoose))
    - Add $text to the $match stage in GET /contests ([https://www.mongodb.com/docs/manual/tutorial/text-search-in-aggregation/](https://www.mongodb.com/docs/manual/tutorial/text-search-in-aggregation/))
    - Add $text to the *find* filter in GET /contests/hosted
    
    Allow users to Bookmark/Favorite certain contests and missions.
    
    **Done!**
    
    - Add a DocReference schema that includes refId and refType properties (see Notifications).
    - Add to the User’s schema a *favorites* property (DocReference[]).
    - Add to *routers/users.js* PATCH /user/favorites endpoint having request body contain the following:
    
    ```jsx
    {
    	"add": [
    		{ 
    			"name": String, 
    			"id": Contest._id | Mission._id, 
    			"type": 'Contest' | 'Mission'
    		}
    	],
    	"remove": [
    		NamedReference._id
    	]
    }
    ```
    
    Automatically put the Host into the list of judges for a mission
    
    **Done!**
    
    - Add host to judges when creating mission: POST /contest/id/mission.
    
    When a user leaves a contest, delete their data after a certain amount of time.
    
    **Done!**
    
    - Add a property to Contest schema named inactiveContestants.
    - When a user leaves a contest via  PATCH /contest/:id/contestants
        - Remove the contest from the user’s participatingContests array.
        - Move the user from the Contest’s contestants array to the Contest’s inactiveContestants array.
        - Delete any mission submissions that have not been adjudicated.
    - When a user joins a public contest via PATCH /contest/:id/contestants or accepts an invitation via PATCH /contest/:id/contestants/invitations or is accepted into a private contest via PATCH /contest/:id/contestants/participation-requests
        - Check if the user is in the Contest’s inactiveContestants array.  If so, move the user to the Contest’s contestants array.
        - Add the Contest to the user’s participatingContests array.
    
    Add new validation type that allows a judge to submit a list of userIds for those who have completed the mission.
    
    **Done!**
    
    - Add *UserListUpload* to validationMethods in Mission schema.
    - Add PATCH /contest/:cid/mission/:mid/user-list-upload route to routers/contests/missions/adjudications.js
    
- July 10 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    - Bug: Fix contest searching page numbers not working correctly
    - Invite contestants to a private contest
    - Accept/Deny Invitation to contest
    - Accept/Deny invitation to judge
    - (Archived) Contest Leaderboard
    - (Archived) Delete Mission(s)
    - Server request and functionality for TextUpload mission submission
    - SingleFileUpload for mission submission
    - Fix list of pending requests to support new data structure
    - Fix pending requests selectable list (doesn’t accurately show which users you want to accept or reject)
    - Fix request to join a contest button so that it supports the new data structure
    - Notes:“Create Mission”, clean up UI of search_contests, accountNotFound when inviting to contest
    
    ---
    
    <aside>
    💡 Back End
    
    </aside>
    
    - **Done** Allow contest host to remove **judge invitation** from a mission with the host providing a notification message.
        - Added DELETE /contest/:cid/mission/:mid/judges/invitations.  Endpoint should expect the following object in the body of the request.
        
        ```jsx
        {
        	"emailAddresses": [email]
        	"message": String (optional)
        }
        ```
        
    
    - **Done By Raiji!** Allow host to remove contestant **invitations** from a contest with the host providing a notification message.
        - Added DELETE /contest/:id/contestants/invitations. Endpoint should expect the following object in the body of the request.
    
    ```jsx
    {
    	"emailAddresses": [email],
    	"message": String (optional)
    }
    ```
    
    - **Done** Allow host to remove **contestants** from a contest and send them a notification with message why they were removed, the message is from the host.
        - Added DELETE /contest/:id/contestants endpoint. Endpoint should expect the following object in the body of the request.
        
        ```jsx
        {
        	"userIds": [UserId],
        	"message": String (optional)
        }
        ```
        
    - **Done** Allow contest host to remove **judges** from a mission with the host/coHost providing a notification message.
        - Added DELETE /contest/:cid/mission/:mid/judges.  Endpoint should expect the following object in the body of the request
        
        ```jsx
        {
        	"userIds": [UserId],
        	"message": String (optional)
        }
        ```
        
    
    - **Done** When a host invites a person to be a judge and the person does not have an account, return the email addresses of the persons without an account in an array named accountNotFound.
        - Modify POST /contest/:cid/mission/:mid/judges/invitations to return the following object.
        
        ```jsx
        {
        	invalid: [],
        	alreadyInvited: [],
        	alreadyJudge: [],
        	newlyInvited: [],
        	**accountNotFound: []**
        }
        ```
        
    
    - **Done** Host can invite/disinvite/remove “coHosts” to a a contest that have access to do host things like invite judges to missions. Things they can’t do: delete the contest and modify the list of cohosts.
        - Create **Cohost** schema in models/cohost.js.
        - Add **invitedToCohost** and **coHost** arrays to Contest model.
        - Add **invitedToCohost** and **********************************cohostingContests********************************** to User model.
        - Add **contest/cohost.js** file.  Include the following:
            - Add POST /contest/:id/cohost/invitations endpoint to allow host to invite cohosts.  Expects an array of email addresses in the request body and returns the following:
            
            ```jsx
            {
            	invalid: [],
            	alreadyInvited: [],
            	alreadyCoHost: [],
            	newlyInvited: [],
            	accountNotFound: []
            }
            ```
            
            - Add DELETE /contest/:id/cohost/invitations endpoint to allow a host to delete a coHost invitation.
            - Add PATCH /contest/:id/cohost/invitations endpoint to allow users to accept or reject invitation to cohost a contest.  Expects “action=accept” or ‘action=reject” in request.
            - Add DELETE /contest/:id/cohost endpoint to allow host to remove any cohost.
            - Add PATCH /contest/:id/cohost to allow a cohost to leave a contest (as cohost)
            - Modify existing endpoints to allow cohosts to perform host duties (except those above).
    
    - **Done** Use email to verify email addresses when creating an account
        - Add **emailVerified** to User model with default set to **false**.
        - Add **emailVerificationToken** to User model.
        - Add **admin@knowledgechase.xyz** email in Godaddy Control Panel
        - Create/setup SendGrid account.
            - Create admin@knowledgechase.xyz email address sender
        - Modify POST /user to send email with link to http://knowledgechase.xyz/account/verification?token=token
        - Modify POST /user/login to return 401 status if emailVerified is false
        - Add POST /user/verification?token=token which checks token and sets emailVerified to true if valid.
        - Add GET /user/verification to resend verification email.
        
        **Done**: We need to verify that the mission start and end dates are between the contest start and end dates.
        
        **Done**: When contestant submits completed mission submission, need to verify time submitted is between start and stop date of mission, else reject submission.
        
- July 24 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    Leaderboard page
    
    Routing for mission page
    
    Clean up mission page to be more user friendly and match new design of Search Contests page
    
    Touch up View Mission
    
    Clean up design on Contest Host Settings page
    
    Invite Cohosts
    
    Accept/Deny/Remove Cohost invitation
    
    Remove Cohosts
    
    Remove contestants
    
    Update/Edit Mission Data
    
    Update layout on view contest page to be more user friendly
    
    Email verification page
    
    Done: Move active header bar section (css)
    
    Disable searching for contests when not logged in
    
    Archived: Beta-testing sign up page
    
    <aside>
    💡 Back End
    
    </aside>
    
    **Done:** Restrict cohosts from using PATCH /contest/:id
    
    **Done**: Add a tally of the submissions submitted by each user to each mission.
    
    - Create **SubmissionsTally** schema.
    
    ```jsx
    SubmissionsTally { 
    	userId: User._id, 
    	pending: Number, 
    	approved: Number, 
    	rejected: Number 
    }
    ```
    
    - Add to Mission model **tallies: SubmissionsTally[]**.
    - Modify POST /contest/:cid/mission/:mid/submission.  When a contestant submits a mission submission a SubmissionTally object is added to tallies or updated.
    - Modify GET /contest/:id to return user’s submissionTally
    - Modify PATCH /contest/:cid/mission/:mid/adjudication/.  When a submission is adjudicated, update SubmissionTally.
    - Modify PATCH /contest/:cid/mission/:mid/submission/:sid.  If submission is changed from rejected to pending, update SubmissionTally.
    - Modify PATCH /contest/:cid/mission/:mid/user-list-upload. When a judge submits uploads a list of contestants a SubmissionTally object is added to tallies or updated to each contestant.
    - Modify DELETE /contest/:cid/mission/:mid/submission/:sid so that the tally is updated when a contestant deletes a pending or rejected submission.
    
    **Done**: Create endpoint to get submissions with pagination and filters. Response includes count. Filters include: skip, status, asc/desc, ~~search (mission title)~~.
    
    - Remove submissions from GET /contests.
    - Create GET /submissions endpoint (see API docs)
    
    **Done**: Send error code when a user tries to login with an unverified account
    
    - Response has status code 401
    
    **Done**: Add option when host creates contest to make themselves a judge of all missions by default or not.
    
    - Add to Contest model **hostJudgesAllMissions: Boolean** (default true).
    - Modify POST /contest/{cid}/mission to set host as judge if hostJudgesAllMissions is true.
    - Modify PATCH /contest/{cid}/mission/{mid} to prevent cohosts from removing host as judge if hostJudgesAllMissions is true.
    
    **Done**: Add option when host/cohost creates mission to make themselves a judge. 
    
    - Add query string (**setUserAsJudge:Boolean** (default true) to POST /contest/{cid}/mission.
    
    **Done**: Allow a host to transfer full ownership of contest to a cohost.
    
    - Create new endpoint: PATCH /contest/{cid}/ownership which takes the cohostId in the request body, sets the cohost as the host, and sets the host as a cohost. Notification is sent to new host.
    
    **Done:** Add some piece of information to be sent back from GET contest so that when a user clicks “Request to Join Contest” and they come back at a later time to view that contest, there is some piece of data such as “requestSent: boolean” to instead display that their request is pending.
    
    - Modify GET /contest/{id} to include the following added property:
        
        ```jsx
        status: {
        	hosting: Boolean,
        	coHosting: Boolean,
        	invitedToCohost: Boolean,
        	invitedToParticipate: Boolean,
        	participationRequestPending: Boolean
        }
        ```
        
    
    **Done**: Make retrieving list of multiple contests also include how many pages possible there are, that way if the user keeps trying to go to the next page the last one won’t be blank, also possibly to provide navigation to go to page: 1, 2, 3, … 5
    
    - GET /contests and GET /contests/hosted returns the following object:
    
    ```jsx
    {
    	contests: [],
    	total: Number
    }
    ```
    
    **Done**: “Search contests that I’m in”, I think this is useful but at the moment the implimentation is getting the userdata then looping through the contest id’s and getting all their data then creating the html list on screen which is not great.
    
    Add query param for private to search all contests so that will include private contest’s that you are a part of and have access to see.
    
    - (modified) Add `private=true|false` as query string to GET /contests.  Omitting private query string will retrieve public and private contests.
    
    **Done**: Add a way to retrieve a contest based on missionId.
    
    - Modify GET /contest/{id} to include query string `idType=mission` (default contest).
    
    **Done**: Add the ability to search for missions outside of being in a contest, in case someone was part of a lot of different contests and wanted to find a particular mission quickly by name and forgot which contest it was in.
    
    - Add GET /missions to allow user to search and retrieve missions.  Allows same query strings as GET /contests, but returns array of missions rather than contests.  Each mission now has `contestId` field that match the value in the parent contest.
    
    ************Done:************ Add a way to set a notification as read.
    
    - Add `read:Boolean` (default false) to Notification model
    - Add PATCH /user/notifications/:id to toggle the `read` value.
    
    **TODO**: Implement controlled release to new users for beta testing and to control costs.
    
    Create a new sign-up page and only grant access to the Create Account page to those who receive invitation to create account email.
    
    - **Create BETA global in app.js**
    - **Create Administrator model { email, password, firstName, lastName, phone }**
    - **Add admin/administrators.js router file with POST /admin, POST /admin/login, POST /admin/logout routes.**
    - Add invitedToCreateAccount property in User model.
    - Create POST /user/signup to accept email address and store them in PotentialUsers.registrants, and send “stay tuned” email.
    - Create POST /user/invitations that can be run only by admin and that takes a list of registrant ids in the body of the request.  For each registrant, the endpoint:
        - creates a tokens and stores it in the registrant doc
        - sends an email to the registrant with a link to the app’s Create Account page along with their token.
    - Modify POST /user.
        - If in BETA mode, require token in request’s query string; If missing, return 401.  If present, create user and set emailVerified to true.
        - If !BETA, send verification email.
    
- August 7 Sprint
    
    <aside>
    💡 Front End
    
    </aside>
    
    ( Important )
    
    - Single File Upload (for contestants) and User List Upload (for hosts) for mission submissions
    - Manage Contestants
        - Remove, Ban, and Unban contestant
    - Invite to contest (Create Invitation to Contest)
    - Evaluate requests to join contest
    - Manage Cohosts (Invite, View, Remove Invitation, Remove Self as Cohost, Delete Cohost)
    - Edit Mission Details
    - **Done:** Resend Verification Email (login page, please check your email)
    - View List of everyone in the Contest with their roles
    - Submissions
        - Modify submission
        - Delete submission
        - Get list of submissions
    - From TODO: Add to each mission small icons with number of accepted, pending, and rejected submissions.
    - Adjudications
        - Adjudicate submission
        - (User List Upload)
    - Notifications
        - Toggle notification read status
        - Accept/Reject Contest Invitation: Done, redirects user to contest on acceptance. Deletes notification
        - Accept/Reject Invitation to Cohost: Done, redirects user to contest on acceptance. Deletes notification
        - Accept/Reject Invitation to Judge: Done, redirects judge to mission on acceptance. Deletes notification
        - Mission available to judge: Done, redirects judge to mission on acceptance
    - **Done**: Index.html page stuff
        - about page, terms of service, privacy policy, FAQ page
        - about page has no content, but is formatted like the others.
    - **Done**: Error Messages (when trying to log in)
    - **Done:** Resend verification email link when receiving 401 on login
    - Add gavel in mission-list next to missions that are judged by the user.
    
    *notes
    
    - **Done:** go to contests page right after login
    - **Done**: potentially change main.html to be a help page
    - flag on search contests page on contests that you’ve been invited to
    - on mobile, user can’t delete notifications that you need to accept or reject
    
    ( Not quite as Urgent )
    
    - Transfer Ownership
    - Add/Delete Favorites
    - Search Missions (without contest)
    
    ---
    
    <aside>
    💡 Back End
    
    </aside>
    
    - **Done:** Max contests user can create
        - Added `maxContestsUserCanHost` to User model with default value 3
        - Modified POST /contest.  Returns 401 if number of contests in hostingContests[] is gte maxContestsUserCanHost.
    - **Done:** Add favorite contests and favorite missions role to search contests
        - Removed `favorites` from User model.  Replaced with `favoriteContests` and `favoriteMissions` arrays.
        - Removed PATCH /user/favorites.  Replaced with PATCH /user/favoriteContests and PATCH /user/favoriteMissions.  Endpoints no longer require `type` when adding favorite contest or mission.
    - **Done**: Add firstName, lastName, and original message to banned users.
        - Changed contest.bannedUsers from User._id[] to Contestant[].
        - Added `reasonForBanishment` to Contestant model
        - Modified POST /contest/:id/bannedUsers to move contestant from contestants to bannedUsers.
        - Modified PATCH /contest/:id/bannedUsers to move contestant from bannedUsers to contestants.
    - **Done**: API sends emails to those who have been invited (to participate, cohost, or judge) but don’t have a KC account.
    - **Done**: Need to not allow invitations to judge to existing judges.  Will also check invitations to cohost and participate.