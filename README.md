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
