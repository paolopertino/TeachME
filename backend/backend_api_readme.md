# Backend API

## Register new user

**Route:** `/register`  
**Methods:** `POST`  
**Description:** Registers a new user in the system.  
**Request parameters:**

- **email (required, string):** email address of the user.
- **username (required, string):** username of the user.
- **password (required, string):** password of the user.
- **role (required, string):** role of the user. Possible values: _teacher_, _student_.

**Expected data format (example):**

```json
POST /register

{
    "email": "teacher@example.com",
    "username": "teacher1234",
    "password": "password1234",
    "role": "teacher"
}
```

**Response:** Returns code 200 if the user was registered correctly.  
**Error handling:** Returns error 400 if a problem occurred while registering the new user.

## Login user

**Route:** `/login`  
**Methods:** `POST`  
**Description:** Logs user into the system.  
**Request parameters:**

- **email (required, string):** email address of the user.
- **password (required, string):** password of the user.

**Expected data format (example):**

```json
POST /login

{
    "email": "teacher@example.com",
    "password": "password1234"
}
```

**Response:** Returns code 200 if the user was logged in correctly.  
**Error handling:** Returns error 400 if a problem occurred while logging the user into the system.

## Get current user information

**Route:** `/me`  
**Methods:** `GET`  
**Description:** Retrieves information about the user currently logged in. If no user is logged in this returns an error.

**Response:** Returns a JSON object containing:

- user_id: The identifier of the logged-in user.
- user_email: The email of the logged-in user.
- role: The role of the logged-in user.

**Error handling:** Returns error 400 if no user is logged in.

## Create friendship

**Route:** `/create-friendship`  
**Methods:** `POST`  
**Description:** Creates a _friendship_ between a teacher and a student in the database.  
**Request parameters:**

- **teacher_email (required, string):** email address of the teacher.
- **student_email (required, string):** email address of the student.

**Expected data format (example):**

```json
POST /create-friendship

{
    "teacher_email": "teacher@example.com",
    "student_email": "student@example.com"
}
```

**Response:** The function returns the simple message "Ok" upon successful creation of the friendship.

## Remove friendship

**Route:** `/remove-friendship`  
**Methods:** `POST`  
**Description:** Removes the _friendship_ between a teacher and a student in the database.  
**Request parameters:**

- **teacher_email (required, string):** email address of the teacher.
- **student_email (required, string):** email address of the student.

**Expected data format (example):**

```json
POST /remove-friendship

{
    "teacher_email": "teacher@example.com",
    "student_email": "student@example.com"
}
```

**Response:** The function returns the simple message "Ok" upon successful removal of the friendship.

## Create conversation

**Route:** `/create-conversation`  
**Methods:** `POST`  
**Description:** Creates a new conversation in the database.  
**Request parameters:**

- **user_level (required, string):** Level of the user. Possible values: _beginner_, _intermediate_, _advanced_.
- **difficulty (required, string):** Difficulty of the conversation. Possible values: _easy_, _medium_, _challenging_.
- **topic (optional, string):** Topic of the conversation. Defaults to None.
- **teacher_email (required, string):** Email address of the teacher who created the conversation.
- **student_email (required, string):** Email address of the student whom the conversation was assigned to.
- **time_limit (optional, string or int):** Time limit of the conversation (in minutes). Defaults to 5 minutes.

**Expected data format (example):**

```json
POST /create-conversation

{
    "user_level": "intermediate",
    "difficulty": "challenging",
    "topic": "Last summer holidays",
    "teacher_email": "teacher@example.com",
    "student_email": "student@example.com",
    "time_limit": 10
}
```

**Response:** The function returns the simple message "Ok" upon successful creation of the conversation.

## Initialize conversation

**Route:** `/initialize-conversation`  
**Methods:** `POST`  
**Description:** Initializes an already existing conversation. This is necessary in order to load the corresponding chatbot from the conversation in the database.  
**Request parameters:**

- **conversation_id (required, string)**: The unique identifier of the conversation.

**Expected data format (example):**

```json
POST /initialize-conversation

{
    "conversation_id": "6645c6ebda20b82cd697390d"
}
```

**Response:** The function returns the code 200 and the message "Conversation initialized successfully" upon successful creation of the conversation. If an error occurs, it returns the code 400 with a message specifying the error.

## Send user message to chatbot

**Route:** `/user-chat-message`  
**Methods:** `POST`  
**Description:** Handles user messages sent to the specified chatbot, through a POST request.  
**Request parameters:**

- **conversation_id (required, string)**: The unique identifier of the conversation.
- **sender_id (required, string)**: The unique identifier of the user who sends the message.
- **message (required, string)**: The text message sent by the user.

**Expected data format (example):**

```json
POST /user-chat-message

{
    "conversation_id": "6645c6ebda20b82cd697390d",
    "sender_id": "43379a1b-f39d-489b-bb9c-8d8adbce6325",
    "message": "Hi, my name is Ciuchino!"
}
```

**Response:** A JSON object with the following properties:

- **conversation_id (string)**: The conversation ID (same as the request parameter).
- **response (string)**: The chatbot's response to the user message.

## Get user's conversations

**Route:** `/list-user-conversations/<user_email>`  
**Methods:** `GET`  
**Description:** Returns the list of all the conversations that the user is involved in. If the user is a teacher, this method returns all the conversations that the user created. If the user is a student, this method returns all the conversations that were assigned to the user.  
**Query parameters:**

- **user_email (required, string):** email address of the user.

**Expected data format (example):** `GET /list-user-conversations/student@example.com`

**Response:** A JSON file containing the ids of all the conversations that the user is involed in.

**Example response:**

> **REQUEST**: /list-user-conversations/paolo@mail.com

```json
[
  {
    "_id": "66448e578aa4c6fea2e853ee",
    "difficulty": "easy",
    "is_ended": false,
    "student_email": "paolo@mail.com",
    "teacher_email": "pietro@mail.com",
    "time_limit": 5,
    "topic": "Hot dogs",
    "user_level": "intermediate"
  },
  {
    "_id": "66448e7df3ccbc576da02ca9",
    "difficulty": "easy",
    "is_ended": false,
    "student_email": "paolo@mail.com",
    "teacher_email": "pietro@mail.com",
    "time_limit": 5,
    "topic": "Hot dogs",
    "user_level": "intermediate"
  }
]
```

**Error handling:** Returns error 400 if a problem occurred.

## Get conversation info

**Route:** `/get-conversation-info/<conversation_id>`  
**Methods:** `GET`  
**Description:** Returns information about the conversation with the specified id.  
**Query parameters:**

- **conversation_id (required, string):** id of the conversation.

**Expected data format (example):** `GET /get-conversation-info/6645c6ebda20b82cd697390d`

**Response:** A JSON containing the information about the conversation with the specified id.  
**Error handling:** Returns error 400 if a problem occurred.

## Get user's friends

**Route:** `/get-friends/<user_email>`  
**Methods:** `GET`  
**Description:** Retrieves all the friends of the user. If the user is a teacher, this returns all the teacher's students. If the user is a student, this returns all the student's teachers.  
**Query parameters:**

- **user_email (required, string):** email address of the user.

**Expected data format (example):** `GET /get-friends/student@example.com`

**Response:** The function returns a JSON containing the friends of the specified user.  
**Error handling:** Returns error 400 if the specified user was not found in the database.

## Get all the students registered to the platform

**Route:** `/get-all-students`  
**Methods:** `GET`  
**Description:** Retrieves all the students registered to the platform, returning a list of dictionaries containing the students' email addresses, usernames and ids.

**Expected data format (example):** `GET /get-all-students`

**Response:** The function returns a JSON containing a list of dictionaries, each containing the email address, username and id of a student.

**Example response:**

> **REQUEST**: /get-all-students

```json
[
    {
        "_id": "cd70b6d8-d393-487b-9faf-aeaa10904349",
        "email": "pie6@mail.com",
        "username": "pietro6"
    },
    {
        "_id": "07c291ac-74e5-4bda-95f2-319800ed70b2",
        "email": "pie7@mail.com",
        "username": "pie7"
    },

    ...,

    {
        "_id": "727a604b-f8e2-4f8d-add4-0406ac375bfa",
        "email": "s@s.com",
        "username": "s"
    }
]
```

## Get username and friends

**Route:** `/get-username/<user_email>`  
**Methods:** `GET`  
**Description:** Retrieves the username and friends of the user with the specified email address.
**Query parameters:**

- **user_email (required, string):** email address of the user.

**Expected data format (example):** `GET /get-username/student@example.com`

**Response:** The function returns a JSON containing the username and friends of the specified user.

**Example response:**

> **REQUEST**: /get-username/pietestpostman_teach@mail.com

```json
{
  "friends": ["pietestpostman@mail.com"],
  "username": "pietestpostman_teach"
}
```

**Error handling:** Returns error 400 if the specified user was not found in the database.

## End Conversation

**Route:** `/end-conversation/<conversation_id>`  
**Methods:** `GET`  
**Description:** Ends the conversation with the specified id. Once a conversation is ended, the user will not be able to conversate with the chatbot in that conversation anymore.  
**Query parameters:**

- **conversation_id (required, string):** id of the conversation to end.

**Expected data format (example):** `GET /end-conversation/6645c6zzzz20b82cd697390d`

**Response:** The function returns a success message upon successful ending of the conversation.
**Error handling:** Returns error 400 if the specified conversation was not found in the database, i.e., if an error occured.

## Get post-conversation feedbacks

**Route:** `/post-conversation-info/<conversation_id>`  
**Methods:** `GET`  
**Description:** Retrieves the post-conversation feedbacks of the conversation with the specified id.
**Query parameters:**

- **conversation_id (required, string):** id of the conversation to get the feedbacks from.

**Expected data format (example):** `GET /post-conversation-info/6645c6zzzz20b82cd697390d`

**Response:** The function returns a JSON containing the post-conversation feedbacks of the specified conversation. A detailed structure of the JSON is provided below.

**Example response:**

> **REQUEST**: /post-conversation-info/6645c6zzzz20b82cd697390d

```json
{
    "_id": "6645c6zzzz20b82cd697390d",
    "messages": [
        {
            "message_content": "I love apples", // content of the message
            "role": "human", // this value could be either 'human' or 'ai'
            "feedback": {
                "hasMistake": false,
                "messageFeedback": ""
            },
            "synonyms": [], // no interesting words for which synonyms should be shown to the user
            "pronunciation": [], // no particularly difficult words to pronounce detected in the user message
        },
        {
            "message_content": "That's wonderful to hear! Apples are not only delicious but also offer numerous health benefits.",
            "role": "ai",
            "feedback": null,  // 'ai' messages do not have a feedback
            "synonyms": null,  // 'ai' messages do not have the synonim challenge
            "pronunciation": null,  // 'ai' messages do not have the pronunciation challenge
        },
        ...,
        {
            "message_content": "The vibrant colors of the orchids added a touch of biuty to the garden.",
            "role": "human",
            "feedback": {
                "hasMistake": true,
                "messageFeedback": "The word beauty has been misspelled. You have said 'biuty'."
            },
            "synonyms": [
                "garden", // the word said by the user while conversating
                "Yard", // 1st possible synonim of the word garden
                "Park", // 2nd possible synonim of the word garden
                "Orchard", // 3rd possible synonim of the word garden
            ],
            "pronunciation": [
                "beauty", // 1st difficult word said by the user that is difficult to pronounce
                ...,
                "orchids" // n-th difficult word said by the user that is difficult to pronounce
            ],
        }
    ],
    "role_reversed_prompt": "", // ignore this field
    "overall_feedback": "Thank you for engaging in this conversation and sharing about your university project success and your passion for tennis. Your enthusiasm for both topics shines through, which is great for building rapport in a conversation.

**Strengths:**
1. **Expressing Achievements:** You effectively communicated your recent success with the university project and the good grade you received, showcasing a positive accomplishment.
2. **Passion for Tennis:** Your love for tennis comes across clearly in your responses, especially when you mentioned enjoying the strategic aspect of the game and your admiration for players like Roger Federer.

**Areas for Improvement:**
1. **Clarity and Detail:** Try to provide more specific details when discussing your experiences. For instance, instead of saying you've been playing tennis since you were a child without remembering the exact date, you could estimate the years or talk about memorable moments in your tennis journey.
2. **Elaboration and Engagement:** When discussing your favorite player, Roger Federer, you briefly mentioned his retirement without expanding on how his style or achievements influenced your love for tennis. Providing more depth to your responses can enrich the conversation.

**Suggestions for Improvement:**
1. **Share Personal Experiences:** Try to share personal anecdotes or specific instances related to playing tennis or watching matches. This can make your responses more engaging and relatable.
2. **Ask Questions:** To keep the conversation dynamic, consider asking your conversational partner questions about their interests or opinions, which can lead to more interactive and enjoyable exchanges.
3. **Expand on Ideas:** When discussing tennis players or tournaments, delve deeper into why you admire certain players or what excites you about current matches. This can add depth to the conversation and invite further discussion.

Overall, your participation in the conversation was positive. By incorporating more details, personal insights, and interactive elements, you can further enhance your communication skills and enrich future conversations. Keep up the good work and continue sharing your experiences and interests with enthusiasm!"
}
```

> **NOTE**: the overall feedback is compiled only when the conversation has been successfully ended.  
> In the case above, the content of the feedback and of the messages are not correlated since the feedback string has been placed only for demonstration purposes. With a true conversation the overall feedback reflects both the content and the structure of the conversation it refers to.

**Error handling:** Returns error 400 if the specified conversation was not found in the database, i.e., if an error occured.

## Activate roles reversed challenge

**Route:** `/create-conversation-roles-reversed`  
**Methods:** `POST`  
**Description:** Creates, starting from the (ended) conversation with the specified id, a new conversation with the roles reversed challenge activated. Note that a conversation with the roles reversed challenge activated is exactly the same as a normal conversation, but with a slightly modified system prompt for the chatbot. Frontend side, there should not be any difference between a normal conversation and a conversation with the roles reversed challenge activated.

**Query parameters:**

- **conversation_id (required, string):** id of the conversation from which to create the new conversation with the roles reversed challenge activated.

**Response:** The function returns a JSON containing the new conversation ID if the creation is successful.

**Error handling:** Returns error 400 if the specified conversation was not found in the database, i.e., if an error occured.