# Data Implementation

## Overview
This document outlines the data Implementation for the CrimeScope Analytics Platform. The model is designed to capture the intricacies of crime data while allowing for comprehensive analysis and reporting. The schema has been implemented in PostgreSQL, and the SQL Data Definition Language (DDL) is used to define the structure of the database.

## Schema Definition
The following is the SQL DDL for creating the necessary tables in our PostgreSQL database. The tables are created in a sequence that respects the dependencies between them, ensuring that foreign key relationships are maintained and that the integrity of the data model is preserved.

```sql
-- Create tables
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

## Schema Definition
```sql
-- Inserting countries
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
