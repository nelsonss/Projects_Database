# Conceptual Design

## Overview

The conceptual design phase of the CrimeScope Analytics Platform was a critical step in laying the groundwork for a system that could seamlessly integrate diverse crime datasets. Our approach was methodical and user-centric, ensuring that the end product would not only be robust in handling complex data but also intuitive for users conducting various levels of crime analysis.

## Analysis of Crime Datasets

Our journey began with an exhaustive analysis of multiple crime datasets from different regions and agencies, including:

- Chicago Crime Dataset
- National Crime Records Bureau (NCRB) - India
- FBI Uniform Crime Reporting (UCR) Program - United States
- Los Angeles Crime Dataset
- Philadelphia Crime Dataset
- UK Police Data
- New York City Crime Data

For each dataset, we identified common fields such as incident date and time, location, type of crime, and involved parties (suspects, victims, and officers). We also noted unique attributes that could provide deeper insights, such as the method of crime, response times, and outcomes of incidents.

## Design Goals

The design goals for the CrimeScope Analytics Platform were established as follows:

1. **Data Compatibility**: To create a unified structure capable of accommodating the various data types found in global crime datasets.
2. **User Experience**: To design a user-friendly interface that simplifies the complexity of the underlying data, allowing users to perform complex queries with ease.
3. **Analytical Depth**: To ensure the system supports deep analytical capabilities, enabling users to uncover patterns and trends in crime data.
4. **Flexibility**: To allow for the addition of new datasets and attributes as crime reporting evolves.

## Conceptual Model

The conceptual model for the CrimeScope Analytics Platform was developed with the following considerations:

1. **Entity Identification**: We identified core entities such as `Incident`, `Location`, `Officer`, `Suspect`, `Victim`, and `Crime Type`, each with its own set of attributes.
2. **Relationship Mapping**: We mapped relationships between entities, recognizing the many-to-many relationships that exist in crime data (e.g., multiple suspects or victims involved in a single incident).
3. **Attribute Selection**: We selected attributes for each entity that would provide the most value for analysis while ensuring data integrity and consistency.

## User Interface Considerations

In designing the user interface, we focused on:

1. **Simplicity**: A clean and straightforward layout that allows users to navigate the system without overwhelming them with information.
2. **Query Building**: An intuitive query builder that guides users through the creation of complex queries without requiring advanced technical knowledge.
3. **Visualization Tools**: Incorporation of visualization tools such as maps and charts to help users interpret data patterns and trends.
## Using DBML

Quick and simple free tool to help you draw your database relationship diagrams and flow quickly using simple DSL language
// Use DBML to define your database structure
// Docs: https://dbml.dbdiagram.io/docs
Table Incident {
  IncidentID int [pk, increment] // primary key with auto increment
  Date date
  Time time
  IncidentTypeID int [ref: > IncidentType.IncidentTypeID] // foreign key reference
  LocationID int [ref: > Location.LocationID] // foreign key reference
  ReportStatusID int [ref: > ReportStatus.ReportStatusID] // foreign key reference
}

Table IncidentType {
  IncidentTypeID int [pk, increment]
  Description varchar
  CategoryID int [ref: > Category.CategoryID]
}

Table Category {
  CategoryID int [pk, increment]
  Name varchar
}

Table Location {
  LocationID int [pk, increment]
  Address varchar
  DistrictID int [ref: > District.DistrictID]
  Latitude float
  Longitude float
}

Table District {
  DistrictID int [pk, increment]
  Name varchar
  CityID int [ref: > City.CityID]
}

Table City {
  CityID int [pk, increment]
  Name varchar
  StateID int [ref: > State.StateID]
}

Table State {
  StateID int [pk, increment]
  Name varchar
  CountryID int [ref: > Country.CountryID]
}

Table Country {
  CountryID int [pk, increment]
  Name varchar
}

Table ReportStatus {
  ReportStatusID int [pk, increment]
  Status varchar
}

Table Suspect {
  SuspectID int [pk, increment]
  Name varchar
  Age int
  Gender varchar
  EthnicityID int [ref: > Ethnicity.EthnicityID]
}

Table Ethnicity {
  EthnicityID int [pk, increment]
  Description varchar
}

Table Victim {
  VictimID int [pk, increment]
  Name varchar
  Age int
  Gender varchar
  EthnicityID int [ref: > Ethnicity.EthnicityID]
}

Table Officer {
  OfficerID int [pk, increment]
  Name varchar
  RankID int [ref: > Rank.RankID]
}

Table Rank {
  RankID int [pk, increment]
  Title varchar
}

// Junction table for many-to-many relationship between Suspect and Incident
Table IncidentSuspect {
  IncidentID int [ref: > Incident.IncidentID] // foreign key to Incident table
  SuspectID int [ref: > Suspect.SuspectID] // foreign key to Suspect table
  Role varchar // additional information about the suspect's role in the incident
  Indexes {
    (IncidentID, SuspectID) [pk] // composite primary key
  }
}

// Junction table for many-to-many relationship between Victim and Incident
Table IncidentVictim {
  IncidentID int [ref: > Incident.IncidentID] // foreign key to Incident table
  VictimID int [ref: > Victim.VictimID] // foreign key to Victim table
  Description varchar // additional information about the victim's involvement
  Indexes {
    (IncidentID, VictimID) [pk] // composite primary key
  }
}

// Junction table for many-to-many relationship between Officer and Incident
Table IncidentOfficer {
  IncidentID int [ref: > Incident.IncidentID] // foreign key to Incident table
  OfficerID int [ref: > Officer.OfficerID] // foreign key to Officer table
  ResponseRole varchar // additional information about the officer's role
  Indexes {
    (IncidentID, OfficerID) [pk] // composite primary key
  }
}

## Conclusion

The conceptual design phase set a strong foundation for the CrimeScope Analytics Platform. By thoroughly analyzing existing crime datasets and focusing on user experience, we crafted a conceptual model that balances the intricacies of crime data with the practical needs of users. This phase was pivotal in ensuring that the subsequent stages of development would align with our vision of creating a comprehensive, user-friendly crime analysis platform.

