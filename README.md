# api_testing

The purpose of the RaceDay System is to provide a simple way to manage running events and the people taking part in them.

The system allows:

Organisers to create and manage events.
Organisers to create race categories.
Participants to register for the system.
Participants to enrol in race categories.
Organisers to record race results.
Users to view available events and results.

The system uses Microsoft SQL Server for the database and follows a relational database structure.

2.1 Organisers

Stores information about people responsible for organising RaceDay events.

Main fields:

OrganiserID – Primary Key
FirstName
LastName
Email
Phone

Each organiser can manage multiple events.

2.2 Events

Stores information about RaceDay running events.

Main fields:

EventID – Primary Key
OrganiserID – Foreign Key
EventName
EventDate
Location
Description

Each event belongs to one organiser

2.3 Categories

Stores the different race categories available within an event.

Main fields:

CategoryID – Primary Key
EventID – Foreign Key
CategoryName
DistanceKM
EntryFee

Each event can have multiple categories.

2.4 Participants

Stores information about people participating in the races.

Main fields:

ParticipantID – Primary Key
FirstName
LastName
Email
Phone
DateOfBirth

Each participant can enrol in multiple race categories.


2.5 Enrolments

Stores participant registrations for race categories.

Main fields:

EnrolmentID – Primary Key
ParticipantID – Foreign Key
CategoryID – Foreign Key
EnrolmentDate
PaymentStatus

The Enrolments table connects participants with categories.

2.6 Results

Stores the results achieved by participants.

Main fields:

ResultID – Primary Key
EnrolmentID – Foreign Key
FinishTime
Position
ResultStatus

Each enrolment can have one result.

Relationships
One organiser can manage many events.
One event can contain many categories.
One participant can have many enrolments.
One category can have many enrolments.
One enrolment can have one result.


API Endpoint Plan

The RaceDay API provides endpoints for authentication, profiles, events, categories, enrolments and results.

The API uses JSON for request and response data.

5.1 Authentication
POST /api/auth/register

Description: Register a new participant.

Role: Public

Request:

{
  "firstName": "Sipho",
  "lastName": "Dlamini",
  "email": "sipho@gmail.com",
  "password": "Password123",
  "phone": "0711234567",
  "dateOfBirth": "1998-05-12"
}

Response – 201 Created:

{
  "message": "User registered successfully",
  "participantId": 1,
  "email": "sipho@gmail.com"
}
POST /api/auth/login

Description: Authenticate a user and provide an access token.

Role: Public

Request:

{
  "email": "sipho@gmail.com",
  "password": "Password123"
}

Response – 200 OK:

{
  "message": "Login successful",
  "token": "sample-access-token",
  "participantId": 1,
  "role": "Participant"
}
5.2 User Profile
GET /api/profile

Description: View the logged-in participant's profile.

Role: Participant

Request: No request body.

Response – 200 OK:

{
  "participantId": 1,
  "firstName": "Sipho",
  "lastName": "Dlamini",
  "email": "sipho@gmail.com",
  "phone": "0711234567",
  "dateOfBirth": "1998-05-12"
}
PUT /api/profile

Description: Update the participant's profile.

Role: Participant

Request:

{
  "firstName": "Sipho",
  "lastName": "Dlamini",
  "email": "sipho@gmail.com",
  "phone": "0719998888",
  "dateOfBirth": "1998-05-12"
}

Response – 200 OK:

{
  "message": "Profile updated successfully"
}
5.3 Events
GET /api/events

Description: View all available RaceDay events.

Role: Public

Request: No request body.

Response – 200 OK:

[
  {
    "eventId": 1,
    "eventName": "Johannesburg City Run",
    "eventDate": "2026-10-10",
    "location": "Johannesburg",
    "description": "Annual city running event"
  },
  {
    "eventId": 2,
    "eventName": "Pretoria Spring Race",
    "eventDate": "2026-10-24",
    "location": "Pretoria",
    "description": "Spring running event"
  }
]
GET /api/events/{eventId}

Description: View details of one event.

Role: Public

Example:

GET /api/events/1

Response – 200 OK:

{
  "eventId": 1,
  "organiserId": 1,
  "eventName": "Johannesburg City Run",
  "eventDate": "2026-10-10",
  "location": "Johannesburg",
  "description": "Annual city running event"
}
POST /api/events

Description: Create a new RaceDay event.

Role: Organiser

Request:

{
  "eventName": "Durban Beach Run",
  "eventDate": "2026-11-21",
  "location": "Durban",
  "description": "Annual beach running event"
}

Response – 201 Created:

{
  "message": "Event created successfully",
  "eventId": 4
}
PUT /api/events/{eventId}

Description: Update an existing event.

Role: Organiser

Request:

{
  "eventName": "Johannesburg City Run Updated",
  "eventDate": "2026-10-10",
  "location": "Johannesburg",
  "description": "Updated annual city running event"
}

Response – 200 OK:

{
  "message": "Event updated successfully"
}
DELETE /api/events/{eventId}

Description: Delete an event.

Role: Organiser

Request: No request body.

Response – 200 OK:

{
  "message": "Event deleted successfully"
}
5.4 Categories
GET /api/events/{eventId}/categories

Description: View all categories for an event.

Role: Public

Response – 200 OK:

[
  {
    "categoryId": 1,
    "categoryName": "Fun Run",
    "distanceKM": 5.00,
    "entryFee": 100.00
  },
  {
    "categoryId": 2,
    "categoryName": "Main Race",
    "distanceKM": 10.00,
    "entryFee": 180.00
  }
]
POST /api/events/{eventId}/categories

Description: Add a category to an event.

Role: Organiser

Request:

{
  "categoryName": "Half Marathon",
  "distanceKM": 21.10,
  "entryFee": 300.00
}

Response – 201 Created:

{
  "message": "Category created successfully",
  "categoryId": 7
}
PUT /api/categories/{categoryId}

Description: Update a race category.

Role: Organiser

Request:

{
  "categoryName": "10 KM Main Race",
  "distanceKM": 10.00,
  "entryFee": 200.00
}

Response – 200 OK:

{
  "message": "Category updated successfully"
}
DELETE /api/categories/{categoryId}

Description: Delete a race category.

Role: Organiser

Request: No request body.

Response – 200 OK:

{
  "message": "Category deleted successfully"
}
5.5 Enrolments
POST /api/enrolments

Description: Enrol a participant into an event category.

Role: Participant

Request:

{
  "categoryId": 1
}

Response – 201 Created:

{
  "message": "Enrolment created successfully",
  "enrolmentId": 1,
  "participantId": 1,
  "categoryId": 1,
  "paymentStatus": "Pending"
}
GET /api/enrolments

Description: View the logged-in participant's enrolments.

Role: Participant

Response – 200 OK:

[
  {
    "enrolmentId": 1,
    "eventName": "Johannesburg City Run",
    "categoryName": "Fun Run",
    "distanceKM": 5.00,
    "entryFee": 100.00,
    "enrolmentDate": "2026-09-01",
    "paymentStatus": "Paid"
  }
]
GET /api/events/{eventId}/enrolments

Description: View participants enrolled in an event.

Role: Organiser

Response – 200 OK:

[
  {
    "enrolmentId": 1,
    "participantId": 1,
    "participantName": "Sipho Dlamini",
    "categoryName": "Fun Run",
    "paymentStatus": "Paid"
  },
  {
    "enrolmentId": 3,
    "participantId": 2,
    "participantName": "Lerato Maseko",
    "categoryName": "Main Race",
    "paymentStatus": "Pending"
  }
]
DELETE /api/enrolments/{enrolmentId}

Description: Cancel an enrolment.

Role: Participant

Request: No request body.

Response – 200 OK:

{
  "message": "Enrolment cancelled successfully"
}
5.6 Results
GET /api/results

Description: View available race results.

Role: Public

Response – 200 OK:

[
  {
    "resultId": 1,
    "participantName": "Sipho Dlamini",
    "eventName": "Johannesburg City Run",
    "categoryName": "Fun Run",
    "finishTime": "00:28:35",
    "position": 15,
    "resultStatus": "Finished"
  }
]
GET /api/results/{resultId}

Description: View a specific race result.

Role: Public

Example:

GET /api/results/1

Response – 200 OK:

{
  "resultId": 1,
  "participantName": "Sipho Dlamini",
  "eventName": "Johannesburg City Run",
  "categoryName": "Fun Run",
  "finishTime": "00:28:35",
  "position": 15,
  "resultStatus": "Finished"
}
POST /api/results

Description: Record a participant's race result.

Role: Organiser

Request:

{
  "enrolmentId": 1,
  "finishTime": "00:28:35",
  "position": 15,
  "resultStatus": "Finished"
}

Response – 201 Created:

{
  "message": "Result recorded successfully",
  "resultId": 1
}
PUT /api/results/{resultId}

Description: Update a race result.

Role: Organiser

Request:

{
  "finishTime": "00:27:55",
  "position": 12,
  "resultStatus": "Finished"
}

Response – 200 OK:

{
  "message": "Result updated successfully"
}


6. API Error Responses

The API should also return clear error messages when a request cannot be completed.

400 Bad Request

Used when the information supplied by the user is invalid.

{
  "error": "Invalid request",
  "message": "Email address is required"
}
401 Unauthorized

Used when the user is not logged in or the authentication token is invalid.

{
  "error": "Unauthorized",
  "message": "Please log in to access this resource"
}
403 Forbidden

Used when the user does not have permission to perform an action.

{
  "error": "Forbidden",
  "message": "Organiser role required"
}
404 Not Found

Used when the requested record does not exist.

{
  "error": "Not Found",
  "message": "Event not found"
}
409 Conflict

Used when the request conflicts with existing data.

{
  "error": "Conflict",
  "message": "Email address is already registered"
}
500 Internal Server Error

Used when an unexpected server error occurs.

{
  "error": "Internal Server Error",
  "message": "An unexpected error occurred"
}


User Roles
Public

A public user can:

Register
Login
View events
View event details
View categories
View race results
Participant

A participant can:

View their profile
Update their profile
Enrol in race categories
View their enrolments
Cancel an enrolment
Organiser

An organiser can:

Create events
Update events
Delete events
Create categories
Update categories
Delete categories
View event enrolments
Record race results
Update race results
9. SQL Server Database

The database is called:

RaceDay

The database is designed for Microsoft SQL Server and can be created using SQL Server Management Studio (SSMS).

The script creates exactly six tables:

Organisers
Events
Categories
Participants
Enrolments
Results

The SQL script includes:

Primary keys
Foreign keys
NOT NULL constraints
UNIQUE constraints
DEFAULT values
Sample data
Relationship testing queries



The database includes sample data to demonstrate that the system works.

Organisers
Thabo Mokoena
Sarah Williams
Events
Johannesburg City Run
Pretoria Spring Race
Cape Town Coastal Run
Categories
Fun Run – 5 KM
Main Race – 10 KM
Coastal Race – 15 KM
Participants
Sipho Dlamini
Lerato Maseko
James Smith
Nomsa Khumalo
Enrolments

Sample enrolments demonstrate participants registering for different race categories.

Payment statuses include:

Paid
Pending
Results

Sample results demonstrate completed races.

Results include:

Finish time
Position
Result status

The database uses constraints to maintain data accuracy.

Primary Keys

Every table has a primary key:

OrganiserID
EventID
CategoryID
ParticipantID
EnrolmentID
ResultID
Foreign Keys

Foreign keys connect the tables:

Events.OrganiserID
Categories.EventID
Enrolments.ParticipantID
Enrolments.CategoryID
Results.EnrolmentID
Unique Values

Email addresses are unique for:

Organisers
Participants

The EnrolmentID in Results is also unique to maintain the one-to-one relationship between an enrolment and a result.

Default Values
PaymentStatus = Pending
ResultStatus = Pending


Project Structure
RaceDay/
│
├── docs/
│   ├── ERD.png
│   ├── API_Endpoint_Plan.pdf
│   └── README.md
│
├── sql/
│   └── RaceDay.sql
│
└── README.md
