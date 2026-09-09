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
