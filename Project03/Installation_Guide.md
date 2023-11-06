# Installation Guide

To avoid execution errors due to foreign key constraints in PostgreSQL, you should create the tables in an order that ensures any given table is created before other tables that reference it. Here's a suggested sequence for the provided schema:

### Create tables that do not have any foreign keys pointing to them:

Country
Ethnicity
Category
Rank
ReportStatus

### Create tables that have foreign keys pointing only to the tables created in step 1:

State
IncidentType
Continue with tables that depend on the ones from the previous steps:
City
District

### Then create tables that depend on the ones from the previous steps:
Location
Now create tables that have foreign keys pointing to the tables created in the previous steps:
Incident

### Create tables that represent entities that can exist independently

(although they have foreign keys, they are not dependent on the Incident table for their existence):
Suspect
Victim
Officer
Finally, create the junction tables that establish many-to-many relationships:
IncidentSuspect
IncidentVictim
IncidentOfficer

Here's the sequence in SQL:

```sql
CREATE DATABASE "CrimeAnalysis_V1"
    WITH
    OWNER = postgres
    ENCODING = 'UTF8'
    LC_COLLATE = 'Spanish_Latin America.1252'
    LC_CTYPE = 'Spanish_Latin America.1252'
    TABLESPACE = pg_default
    CONNECTION LIMIT = -1
    IS_TEMPLATE = False;
```sql

### Create tables
```sql
CREATE TABLE Country (
  CountryID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL
);

CREATE TABLE State (
  StateID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  CountryID INT REFERENCES Country(CountryID)
);

CREATE TABLE City (
  CityID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  StateID INT REFERENCES State(StateID)
);

CREATE TABLE District (
  DistrictID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  CityID INT REFERENCES City(CityID)
);

CREATE TABLE Location (
  LocationID SERIAL PRIMARY KEY,
  Address VARCHAR NOT NULL,
  DistrictID INT REFERENCES District(DistrictID),
  Latitude FLOAT,
  Longitude FLOAT
);

CREATE TABLE Category (
  CategoryID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL
);

CREATE TABLE IncidentType (
  IncidentTypeID SERIAL PRIMARY KEY,
  Description VARCHAR NOT NULL,
  CategoryID INT REFERENCES Category(CategoryID)
);

CREATE TABLE ReportStatus (
  ReportStatusID SERIAL PRIMARY KEY,
  Status VARCHAR NOT NULL
);

CREATE TABLE Incident (
  IncidentID SERIAL PRIMARY KEY,
  Date DATE NOT NULL,
  Time TIME,
  IncidentTypeID INT REFERENCES IncidentType(IncidentTypeID),
  LocationID INT REFERENCES Location(LocationID),
  ReportStatusID INT REFERENCES ReportStatus(ReportStatusID)
);

CREATE TABLE Ethnicity (
  EthnicityID SERIAL PRIMARY KEY,
  Description VARCHAR NOT NULL
);

CREATE TABLE Suspect (
  SuspectID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  Age INT,
  Gender VARCHAR,
  EthnicityID INT REFERENCES Ethnicity(EthnicityID)
);

CREATE TABLE Victim (
  VictimID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  Age INT,
  Gender VARCHAR,
  EthnicityID INT REFERENCES Ethnicity(EthnicityID)
);

CREATE TABLE Rank (
  RankID SERIAL PRIMARY KEY,
  Title VARCHAR NOT NULL
);

CREATE TABLE Officer (
  OfficerID SERIAL PRIMARY KEY,
  Name VARCHAR NOT NULL,
  RankID INT REFERENCES Rank(RankID)
);

-- Junction tables for many-to-many relationships
CREATE TABLE IncidentSuspect (
  IncidentID INT REFERENCES Incident(IncidentID),
  SuspectID INT REFERENCES Suspect(SuspectID),
  Role VARCHAR,
  PRIMARY KEY (IncidentID, SuspectID)
);

CREATE TABLE IncidentVictim (
  IncidentID INT REFERENCES Incident(IncidentID),
  VictimID INT REFERENCES Victim(VictimID),
  Description VARCHAR,
  PRIMARY KEY (IncidentID, VictimID)
);

CREATE TABLE IncidentOfficer (
  IncidentID INT REFERENCES Incident(IncidentID),
  OfficerID INT REFERENCES Officer(OfficerID),
  ResponseRole VARCHAR,
  PRIMARY KEY (IncidentID, OfficerID)
);
```


### Inserting test data
```sql
INSERT INTO Country (Name) VALUES ('Colombia');

-- Assuming 'Colombia' has an ID of 1 after insertion
-- Inserting states
INSERT INTO State (Name, CountryID) VALUES ('Bogota', 1);

-- Assuming 'Bogota' has an ID of 1 after insertion
-- Inserting cities
INSERT INTO City (Name, StateID) VALUES ('Bogota D.C.', 1);

-- Assuming 'Bogota D.C.' has an ID of 1 after insertion
-- Inserting districts
INSERT INTO District (Name, CityID) VALUES ('Chapinero', 1);

-- Assuming 'Chapinero' has an ID of 1 after insertion
-- Inserting locations
INSERT INTO Location (Address, DistrictID, Latitude, Longitude) VALUES ('Carrera 13 #53-20', 1, 4.6473, -74.0962);

-- Assuming the location has an ID of 1 after insertion
-- Inserting report statuses
INSERT INTO ReportStatus (Status) VALUES ('Reported'), ('Reviewed'), ('Closed');

-- Assuming 'Reported' has an ID of 1 after insertion
-- Inserting incident types and categories
INSERT INTO Category (Name) VALUES ('Theft'), ('Assault'), ('Traffic');
-- Assuming 'Theft' has an ID of 1 after insertion
INSERT INTO IncidentType (Description, CategoryID) VALUES ('Petty Theft', 1), ('Armed Robbery', 1);

-- Assuming 'Petty Theft' has an ID of 1 after insertion
-- Inserting incidents
INSERT INTO Incident (Date, Time, IncidentTypeID, LocationID, ReportStatusID) VALUES ('2023-11-01', '08:00:00', 1, 1, 1);

-- Assuming the incident has an ID of 1 after insertion
-- Inserting ethnicities
INSERT INTO Ethnicity (Description) VALUES ('Hispanic'), ('Caucasian'), ('African American');

-- Assuming 'Hispanic' has an ID of 1 after insertion
-- Inserting suspects
INSERT INTO Suspect (Name, Age, Gender, EthnicityID) VALUES ('John Doe', 30, 'Male', 1);

-- Assuming 'John Doe' has an ID of 1 after insertion
-- Inserting victims
INSERT INTO Victim (Name, Age, Gender, EthnicityID) VALUES ('Jane Smith', 25, 'Female', 1);

-- Assuming 'Jane Smith' has an ID of 1 after insertion
-- Inserting officers and ranks
INSERT INTO Rank (Title) VALUES ('Officer'), ('Sergeant');
-- Assuming 'Officer' has an ID of 1 after insertion
INSERT INTO Officer (Name, RankID) VALUES ('Officer Ryan', 1);

-- Assuming 'Officer Ryan' has an ID of 1 after insertion
-- Inserting junction table data
INSERT INTO IncidentSuspect (IncidentID, SuspectID, Role) VALUES (1, 1, 'Perpetrator');
INSERT INTO IncidentVictim (IncidentID, VictimID, Description) VALUES (1, 1, 'Robbery Victim');
INSERT INTO IncidentOfficer (IncidentID, OfficerID, ResponseRole) VALUES (1, 1, 'Responder');
```
### Perform simple queries to find out basic data
```sql
SELECT *
FROM State;
SELECT * 
FROM City;
SELECT * 
FROM District;
SELECT * 
FROM Location;
SELECT * 
FROM ReportStatus;
SELECT * 
FROM Category;
SELECT * 
FROM IncidentType;
SELECT * 
FROM  Incident;
SELECT * 
FROM Ethnicity;
SELECT * 
FROM  Suspect;
SELECT * 
FROM Victim;
SELECT * 
FROM  Rank;
SELECT * 
FROM  Officer;
SELECT * 
FROM  IncidentSuspect;
```
