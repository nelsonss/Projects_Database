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


