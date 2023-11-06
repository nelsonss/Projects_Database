# Installation Guide

To avoid execution errors due to foreign key constraints in PostgreSQL, you should create the tables in an order that ensures any given table is created before other tables that reference it. Here's a suggested sequence for the provided schema:

## Create tables that do not have any foreign keys pointing to them:

Country
Ethnicity
Category
Rank
ReportStatus

## Create tables that have foreign keys pointing only to the tables created in step 1:

State
IncidentType
Continue with tables that depend on the ones from the previous steps:
City
District

## Then create tables that depend on the ones from the previous steps:
Location
Now create tables that have foreign keys pointing to the tables created in the previous steps:
Incident

## Create tables that represent entities that can exist independently

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

## Create tables
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




