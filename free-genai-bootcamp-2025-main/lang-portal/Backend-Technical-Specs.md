##Technical Specs 
## Business Goal: 
A language learning school wants to build a prototype of learning portal which will act as three things:
Inventory of possible vocabulary that can be learned
Act as a  Learning record store (LRS), providing correct and wrong score on practice vocabulary
A unified launchpad to launch different learning apps


## Technical Requirement
- The backend will be built using Go
- The database will be SQLlite3
- The API will be built using GIN
- The API will always return JSON 
- There will be no authentication or authorization.
- Everything will be treated as a single user.




## Database Schema 

Our database will be single SQLlite database called `words.db`
that will be in the root of the project folder of `backend_go`.

We have the following tables:
- words - stored vocabulary words
  - id  integer
  - japanese string
  - romaji string
  - english string 
  - parts json
- words_groups - join tables for words and groups many-to-many
   - id integer 
   - word_id integer
   - group_id integer

- groups - thematic groups of words
   - id integer 
   - name string
- study_sessions - records of study sessions grouping word_review_items
   - id integer
   - group_id integer 
   - created_at datetime
   - study_activity_id integer
- study_activities - a specific study activity, linknig a study session to a group
   - id integer
   - study_session_id integer
   - group_id integer
   - created_at datetime
- word_review_items - records of word practice, whether determining if the word was correct or not. 
   - word_id integer
   - study_session_id integer 
   - correct boolean 
   - created_at datetime


### API Endpoints

#### Dashboard Endpoints

#### GET /api/dashboard/last_study_session

#### JSON Response
  ```json
{
    
   "id": 123,
   "group_id": 456,
   "created_at": "2025-02-17T08:07:07+11:00",
   "study_activity_id": 789,
   "group_id": 456,
   "group_name": "Basic Greetings"
    
}
```

#### GET /api/dashboard/study_progress
  
#### JSON Response

```json
{
    "total_words_studied": 3,
    "total_available_word" : 124
}
```

- GET /api/dashboard/quick_stats
  ```json
  {
    "success_rate": 80.0,
    "total_study_sessions": 4,
    "total_active_groups": 2,
    "study_streak_days": 3
  }
  
  ```

#### Study Activities Endpoints

- GET /api/study_activities/:id
  ```json
  {
    "id": 123,
    "name" : "Vocabulary Quiz", 
    "thumbnail_url": "https://example.com/thumbnail.png",
    "description" : "Practise your vocabulary with flashcards."
  }
  ```

- GET /api/study_activities/:id/study_sessions
  ```json
  {
    "items": [
      {
        "id": 789,
        "activity_name" : "Vocabulary Quiz",
        "group_name" : "Basic Greetings",
        "start_time": "2025-02-17T08:07:07+11:00",
        "end_time": "2025-02-17T08:07:07+11:00",
        "review_items_count" : 20
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 100,
      "items_per_page": 20
    }
  }
  ```

### POST /api/study_activities 
  - Required params: group_id, study_activity_id
  ```json
  // Request body
  {
    "group_id": 456,
    "study_activity_id": 789
  }
  // Response
  {
    "id": 123,
    "group_id": 456,
    "study_activity_id": 789,
    "created_at": "2025-02-17T08:07:07+11:00"
  }
  ```

#### Words Endpoints

### GET /api/words
  ```json
  {
    "items": [
      {
        "id": 1,
        "japanese": "こんにちは",
        "romaji": "konnichiwa",
        "english": "hello",
        "parts": {
          "type": "greeting",
          "level": "basic"
        }
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "per_page": 100,
      "items_per_page": 100
    }
  }
  ```

### GET /api/words/:id
  ```json
  {
    "id": 1,
    "japanese": "こんにちは",
    "romaji": "konnichiwa",
    "english": "hello",
    "stats": {
      "correct_count": 5,
      "wrong_count": 2
    },
    "groups": [
      {
      "id": 123,
      "name": "Basic Greetings"
    }
    ]
  }
  ```

#### Groups Endpoints

#### GET /api/groups
  ```json
  {
    "groups": [
      {
        "id": 1,
        "name": "Basic Greetings",
        "word_count": 20,
         
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_page": 1,
      "total_items":10,
      "items_per_page": 100 
    }
  }
  ```
### GET /api/groups/:id
  ```json
  {
    "id": 1,
    "name": "Basic Greetings",
    "stats": {
      "total_words": 20
    }
  }
  ```

#### GET /api/groups/:id/words
  ```json
  {
    "items": [
      {
        "japanese": "こんにちは",
        "romaji": "konnichiwa",
        "english": "hello",
        "correct_count": 5,
        "wrong_count" : 2
        }
    ],
    "pagination": {
      "current_page": 1,
      "total_page": 1,
      "total_items": 20,
      "items_per_page": 100
    }
  }
  ```

#### GET /api/groups/:id/study_sessions
  ```json
  {
    "items": [
      {
        "id": 123,
        "activity_name": "Vocabulary Quiz",
        "group_name": "Basic Greetings",
        "start_time": "2025-02-17T08:07:07+11:00",
        "end_time": "2025-02-17T08:07:07+11:00",
        "review_items_count" : 20
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 1,
      "total_items": 5,
      "items_per_page":100

    }
  }
  ```

#### Study Sessions Endpoints

#### GET /api/study_sessions
  ```json
  {
    "items": [
      {
        "id": 123,
        "group_id": 456,
        "group_name": "Basic Greetings",
        "start_time": "2025-02-17T08:07:07+11:00",
        "end_time": "2025-02-17T08:07:07+11:00",
        "review_items_count": 20
      }
    ],
    "pagination": {
       "current_page": 1,       
       "total_pages" : 1,
       "total_items": 5,
       "items_per_page": 100
    }
  }
  ```

- GET /api/study_sessions/:id
  ```json
  {
    "id": 123,
    "activity_name": "Vocabulary Quiz",
    "group_name": "Basic Greetings",
    "start_time": "2025-02-17T08:07:07+11:00",
    "end_time": "2025-02-17T08:07:07+11:00",
    "review_items_count": 20
  }
  
  ```

#### GET /api/study_sessions/:id/words
  ```json
  {
  
    "items": [
      {
        "japanese": "こんにちは",
        "romaji": "konnichiwa",
        "english": "hello",
        "correct_count": 5,
        "wrong_count": 2
        }
    ]
  }
  ```

#### System Management Endpoints
### POST /api/reset_history
  ```json
  {
    "success": true,
    "message": "Study history has been reset successfully"
  }
  ```

- POST /api/full_reset
  ```json
  {
    "success": true,
    "message": "System has been fully reset successfully"
  }
  ```

#### POST /api/study_sessions/:id/words/:word_id/review
#### Request params: 
     - correct boolean 
     - word_id integer
     - study_session_id integer
  
  
#### Request Payload  
```json
  {
    "correct": true

  }

#### JSON Response

  {
    
    "success" : true
    "word_id": 456,
    "study_session_id": 789,
    "correct": true,
    "created_at": "2025-02-17T08:07:07+11:00"
  }
  ```


## Mage Tasks 

Mage is  atask runner for Go. 
Lets list out possible tasks we need for our lang-portal

### Initialize Database 

This task will initialise the sqllite database called 'words.db'


### Migrate Database 

This task will run a series of migrations sql files on the database. 

Migrations live in the `migrations` folder.
The migration files will be run in order of their file name. 

The file names should look like this: 

```sql 

0001_init.sql 
0002_create_words_table.sql
```



#### Seed Data 
This task wil import json files and transform them into target data for our database.


All seed files live in the `seeds` folder 
All seed files should be loaded. 

In our task we should have DSL to specific each seed file and its expected group word name.




``` json 

[
  {
    "kanji": "払う",
    "romaji": "harau",
    "english": "to pay",
  }



]



