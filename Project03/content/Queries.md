# Queries

## Overview
This document describes the SQL queries developed for the CrimeScope Analytics Platform. These queries range from simple data retrieval to complex analytical operations, enabling users to extract meaningful insights from the crime data. They support both operational and strategic decision-making processes.

## Simple Queries
Simple queries are designed to retrieve basic information from the database without complex joins or aggregations.

### Retrieve All Incidents
```sql
SELECT * FROM Incident;
```
### Retrieve all incidents from a specific district:
```sql
SELECT * FROM Incident
JOIN Location ON Incident.LocationID = Location.LocationID
JOIN District ON Location.DistrictID = District.DistrictID
WHERE District.Name = 'Chapinero';
```

### List of Suspects in a Specific Incident
```sql
SELECT Suspect.Name
FROM Suspect
JOIN IncidentSuspect ON Suspect.SuspectID = IncidentSuspect.SuspectID
WHERE IncidentSuspect.IncidentID = 1;
```
### Count the number of incidents by type:
```sql
SELECT IncidentType.Description, COUNT(*) as IncidentCount
FROM Incident
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
GROUP BY IncidentType.Description
```

##  Intermediate Queries
Intermediate queries involve some level of complexity, such as joins across multiple tables or basic aggregations.

### Number of Incidents per District
```sql
SELECT District.Name, COUNT(Incident.IncidentID) AS IncidentCount
FROM Incident
JOIN Location ON Incident.LocationID = Location.LocationID
JOIN District ON Location.DistrictID = District.DistrictID
GROUP BY District.Name;
```
### Retrieve the number of incidents by district and category:
```sql
SELECT District.Name, Category.Name, COUNT(*) as IncidentCount
FROM Incident
JOIN Location ON Incident.LocationID = Location.LocationID
JOIN District ON Location.DistrictID = District.DistrictID
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
JOIN Category ON IncidentType.CategoryID = Category.CategoryID
GROUP BY District.Name, Category.Name;
```

### Details of Incidents Involving Multiple Suspects
```sql
SELECT Incident.IncidentID, STRING_AGG(Suspect.Name, ', ') AS Suspects
FROM Incident
JOIN IncidentSuspect ON Incident.IncidentID = IncidentSuspect.IncidentID
JOIN Suspect ON IncidentSuspect.SuspectID = Suspect.SuspectID
GROUP BY Incident.IncidentID
HAVING COUNT(Suspect.SuspectID) > 1;
```
### Find the average age of victims for each type of incident
```sql
SELECT IncidentType.Description, AVG(Victim.Age) as AverageVictimAge
FROM Incident
JOIN IncidentVictim ON Incident.IncidentID = IncidentVictim.IncidentID
JOIN Victim ON IncidentVictim.VictimID = Victim.VictimID
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
GROUP BY IncidentType.Description;
```
### List incidents that have more than one suspect:
```sql
SELECT Incident.IncidentID, COUNT(IncidentSuspect.SuspectID) as SuspectCount
FROM Incident
JOIN IncidentSuspect ON Incident.IncidentID = IncidentSuspect.IncidentID
GROUP BY Incident.IncidentID
HAVING COUNT(IncidentSuspect.SuspectID) > 1;
```
 
##  Advanced Queries

Advanced queries are the most complex, often involving multiple join operations, subqueries, complex aggregations, and analytical functions.

### Trend of Crime Over Years
```sql
SELECT EXTRACT(YEAR FROM Incident.Date) AS Year, COUNT(*) AS TotalIncidents
FROM Incident
GROUP BY Year
ORDER BY Year;
```
### Find the top 5 districts with the highest number of incidents:
```sql
WITH DistrictIncidents AS (
    SELECT District.Name, COUNT(*) as IncidentCount
    FROM Incident
    JOIN Location ON Incident.LocationID = Location.LocationID
    JOIN District ON Location.DistrictID = District.DistrictID
    GROUP BY District.Name
)
SELECT Name, IncidentCount
FROM DistrictIncidents
ORDER BY IncidentCount DESC
LIMIT 5
```
### Rank officers by the number of incidents they have attended:
```sql
SELECT Officer.Name, COUNT(*) as IncidentsAttended,
RANK() OVER (ORDER BY COUNT(*) DESC) as Rank
FROM IncidentOfficer
JOIN Officer ON IncidentOfficer.OfficerID = Officer.OfficerID
GROUP BY Officer.Name;
```
### Determine the monthly trend of a specific type of crime:
```sql
SELECT EXTRACT(YEAR FROM Date) as Year, EXTRACT(MONTH FROM Date) as Month, COUNT(*) as IncidentCount
FROM Incident
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
WHERE IncidentType.Description = 'Burglary'
GROUP BY Year, Month
ORDER BY Year, Month;
```

### Correlation Between Crime Type and Time of Day
```sql
SELECT IncidentType.Description, EXTRACT(HOUR FROM Incident.Time) AS Hour, COUNT(*) AS IncidentCount
FROM Incident
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
GROUP BY IncidentType.Description, Hour
ORDER BY IncidentType.Description, Hour;
```
