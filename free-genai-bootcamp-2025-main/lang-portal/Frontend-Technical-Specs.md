## Frontend Technical Specs

## Pages 

### Dashboard 

The purpose of this page is to provide summary of the user's study progress.
url: /dashboard

### Components 
This page contains following components:
- Last Study Session 
   shows last activity used 
   shows when last activity used
   summarizes wrong vs correct from last activity
   has a link to group 
- Study Progress 
   -total words
    - across all study sessions show the total words studied out of all possible words in our database. 
   -display mastery progress. eg 0%


- Quick Stats 
  - success rate eg 80%
  - total study sessions eg. 4 
  - total active groups eg. 3
  - study streams eg. 4 days  
- Start Studying Button
  - goes to study activities page

## Needed API endpoints:
- GET /api/dashboard/last_study_session
- GET /api/dashboard/study_progress
- GET /api/dashboard/quick_stats


### Study Activities  /study_activities

The purpose of this page is to show collection of study activities with a thumbnail and its name, to either launch or view the study activity. 

### Components 

- Study Activity Card
  - shows thumbnail of study activity
  - the name of the study activity
  - launch button to take us to launch page 
  - the view page to view more information about past study sessions for this study activity. 


### Needed API endpoints:
- GET /api/study_activities
  - pagination 

### Study Activity Show '/study_activities/:id'

###Purpose 
The purpose of this page is to show past study session and activity

### Components 

- name of the study activity
- THumbnail of the study activity
- Description of the study activity
- Launch button to launch the study activity
- Study activity paginated list
    - id 
    - activity_name
    - group name 
    - start time 
    - end time ( inferred by last word_review_item.)
    - number of review item. 

### Needed API endpoints:
- GET /api/study_activities/:id
- GET /api/study_activities/:id/study_sessions
 

### Study Activity Launch '/study_activities/:id/launch'

### Purpose 
The purpose of this page is to launch the study activity.

### Components 

- Name of the study activity
- Launch form 
 - select field for group 
 - launch now button


 ## Behavior 
After form is submitte a new tab opens with the study activity based on its URL provided in the database. 

Also the after form is submitted the page rediects to study show page. 


 ### Needed API endpoints:

- POST /api/study_activities
  - group_id
  - study_activity_id


### Words '/words'

### Purpose 
The purpose of this page is to show all words in the database. 

### Components 

- Paginated Word List
  -Columns:
   - Japanese
   - Romaji
   - English
   - Correct count
   - Wrong count
   - Actions (edit, delete)
  - Pagination with 100 items per page.
  - Clicking the Japanese word takes you to the word show page. 

  ### Needed API endpoints:

  - GET /api/words 

### Word Show '/words/:id'

### Purpose 
The purpose of this page is to show info about a specific word. 

### Components 
 - Japanese
 - Romaji
 - English
 - Study Statistics
  - Correct count
  - Wrong Count
- Word Groups 
  - show as a series of pills eg. tags
  - when group name is clicked it will take us to the group show page. 

### Needed API endpoints:

  - GET /api/words/:id


### Word Groups '/groups'

### Purpose 
The purpose of this page is to show a list of groups in the database. 

### Components 
- Paginated Group List
  - Columns:
     - Group name
     - Word Count 
  - Clicking the groups name will take us to the group show page.


### Needed API endpoints:

  - GET /api/groups


### Group Show '/groups/:id'

### Purpose 
The purpose of this page is to show info about a specific group. 

### Components 
- Group Name
- Group Statistics
  - Word Count
- Words in Group ( Paginated list of words)
  - Should use the same component as the words index page. 
- Study Sessions ( Paginated list of study sessions)
  - Should use the same component as the study activities index page. 

### Needed API endpoints:
- GET /api/groups/:id ( the name and the groups stats)
- GET /api/groups/:id/words 
- GET /api/groups/:id/study_sessions

### Study Session Index '/study_sessions/:id'


### Purpose 
The purpose of this page is to show list of study sessions in our database. 

### Components 
  - Paginated Study Session List
    - Columns:
      - Id
      - Activity Name
      - Group Name
      - Start Time
      - End Time
      - Number of Review Items
    - Clicking on the study session will take us to the study session show page. 


### Needed API endpoints:
- GET /api/study_sessions



### Study Session Show '/study_sessions/:id'

### Purpose 
The purpose of this page is to show info about a specific study session. 

### Components 
- Study Session Details: 
  - Activity Name
  - Group Name
  - Start Time
  - End Time
  - Number of Review Items
- Word Review Items ( Paginated list of word review items)
  - should use the same component as the words index page. 



### Needed API endpoints:

- GET /api/study_sessions/:id
- GET /api/study_sessions/:id/words


### Settings '/settings'

### Purpose 
The purpose of this page is to make config to the study portal. 

### Components 
 - Theme selection eg light and dark mode. 
 - Reset History Button
   - Will delete all the study history of the user. 

 - Full ResetButton
    - will drop all tables and recreate seed data. 

### Needed API endpoints:

- POST /api/reset_history
- POST /api/full_reset
- POST /api/study_sessions/:id/words/:word_id/review
   - required params: correct 

