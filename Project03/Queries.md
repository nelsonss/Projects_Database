# Queries

## Overview
This document describes the SQL queries developed for the CrimeScope Analytics Platform. These queries range from simple data retrieval to complex analytical operations, enabling users to extract meaningful insights from the crime data. They support both operational and strategic decision-making processes.

## Simple Queries
Simple queries are designed to retrieve basic information from the database without complex joins or aggregations.

### Retrieve All Incidents
```sql
SELECT * FROM Incident;
```

### List of Suspects in a Specific Incident
```sql
SELECT Suspect.Name
FROM Suspect
JOIN IncidentSuspect ON Suspect.SuspectID = IncidentSuspect.SuspectID
WHERE IncidentSuspect.IncidentID = 1;
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

### Details of Incidents Involving Multiple Suspects
```sql
SELECT Incident.IncidentID, STRING_AGG(Suspect.Name, ', ') AS Suspects
FROM Incident
JOIN IncidentSuspect ON Incident.IncidentID = IncidentSuspect.IncidentID
JOIN Suspect ON IncidentSuspect.SuspectID = Suspect.SuspectID
GROUP BY Incident.IncidentID
HAVING COUNT(Suspect.SuspectID) > 1;
```

##  Advanced Queries

Advanced queries are the most complex, often involving multiple join operations, subqueries, complex aggregations, and analytical functions.

### Trend of Crime Over Years
```sql
SELECT EXTRACT(YEAR FROM Incident.Date) AS Year, COUNT(*) AS TotalIncidents
FROM Incident
GROUP BY Year
ORDER BY Year;

### Correlation Between Crime Type and Time of Day
```sql
SELECT IncidentType.Description, EXTRACT(HOUR FROM Incident.Time) AS Hour, COUNT(*) AS IncidentCount
FROM Incident
JOIN IncidentType ON Incident.IncidentTypeID = IncidentType.IncidentTypeID
GROUP BY IncidentType.Description, Hour
ORDER BY IncidentType.Description, Hour;
