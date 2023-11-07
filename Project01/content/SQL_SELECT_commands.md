# Queries

| Query Description (English) | SQL Equivalent |
| --------------------------- | -------------- |
| Get all corruption cases | `SELECT * FROM CorruptionCase;` |
| Get all individuals involved in a specific corruption case | `SELECT Individual.* FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id WHERE Involvement.corruption_case_id = ?;` |
| Get all organizations involved in a specific corruption case | `SELECT Organization.* FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id WHERE Involvement.corruption_case_id = ?;` |
| Get the number of corruption cases each individual is involved in | `SELECT Individual.name, COUNT(Involvement.corruption_case_id) AS case_count FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id GROUP BY Individual.name;` |
| Get the number of corruption cases each organization is involved in | `SELECT Organization.name, COUNT(Involvement.corruption_case_id) AS case_count FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id GROUP BY Organization.name;` |
| Get the most common type of involvement in corruption cases | `SELECT Involvement.involvement_type, COUNT(*) AS count FROM Involvement GROUP BY Involvement.involvement_type ORDER BY count DESC LIMIT 1;` |
| Get the corruption cases involving a specific individual | `SELECT CorruptionCase.* FROM CorruptionCase JOIN Involvement ON CorruptionCase.id = Involvement.corruption_case_id WHERE Involvement.individual_id = ?;` |
| Get the corruption cases involving a specific organization | `SELECT CorruptionCase.* FROM CorruptionCase JOIN Involvement ON CorruptionCase.id = Involvement.corruption_case_id WHERE Involvement.organization_id = ?;` |

Please replace the `?` in the SQL queries with the appropriate values.

| Query Description (English) | SQL Equivalent |
| --------------------------- | -------------- |
| List all corruption cases in a specific location | `SELECT * FROM CorruptionCase WHERE location = ?;` |
| List all corruption cases of a specific type: | `SELECT * FROM CorruptionCase WHERE type = ?;` |
| List all individuals involved in corruption cases of a specific type: | `SELECT Individual.* FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id JOIN CorruptionCase ON Involvement.corruption_case_id = CorruptionCase.id WHERE CorruptionCase.type = ?;` |
| List all organizations involved in corruption cases of a specific type: | `SELECT Organization.* FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id JOIN CorruptionCase ON Involvement.corruption_case_id = CorruptionCase.id WHERE CorruptionCase.type = ?;` |
| Count the number of corruption cases each individual is involved in: | `SELECT Individual.name, COUNT(*) as case_count FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id GROUP BY Individual.name;` |
| Find the individuals who are involved in the most corruption cases: | `WITH individual_case_counts AS ( SELECT Individual.name, COUNT(*) as case_count FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id GROUP BY Individual.name ) SELECT name, case_count FROM individual_case_counts WHERE case_count = (SELECT  MAX(case_count) FROM individual_case_counts);` |
| List all corruption cases and the number of individuals involved in each case: | `SELECT CorruptionCase.id, COUNT(*) as individual_count FROM CorruptionCase JOIN Involvement ON CorruptionCase.id = Involvement.corruption_case_id GROUP BY CorruptionCase.id;` |
| List all individuals and the number of corruption cases they are involved in, ordered by the number of cases | `SELECT Individual.name, COUNT(*) as case_count FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id GROUP BY Individual.name ORDER BY case_count DESC;` |
| List all corruption cases and the number of organizations involved in each case | `SELECT CorruptionCase.id, COUNT(*) as organization_count FROM CorruptionCase JOIN Involvement ON CorruptionCase.id = Involvement.corruption_case_id GROUP BY CorruptionCase.id;` |
| List all organizations and the number of corruption cases they are involved in, ordered by the number of cases: | `SELECT Organization.name, COUNT(*) as case_count FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id GROUP BY Organization.name ORDER BY case_count DESC;` |
| Use a window function to rank individuals by the number of corruption cases they are involved in | `SELECT Individual.name, COUNT(*) OVER (PARTITION BY Individual.name) as case_count FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id;` |
| Use a window function to rank organizations by the number of corruption cases they are involved in: | `SELECT Organization.name, COUNT(*) OVER (PARTITION BY Organization.name) as case_count FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id;` |
| List all pairs of individuals who are involved in the same corruption case | `SELECT i1.name AS individual1, i2.name AS individual2 FROM Involvement AS inv1 JOIN Involvement AS inv2 ON inv1.corruption_case_id = inv2.corruption_case_id JOIN Individual AS i1 ON inv1.individual_id = i1.id JOIN Individual AS i2 ON inv2.individual_id = i2.id WHERE i1.name < i2.name;` |
| List all pairs of organizations that are involved in the same corruption case | `SELECT o1.name AS organization1, o2.name AS organization2 FROM Involvement AS inv1 JOIN Involvement AS inv2 ON inv1.corruption_case_id = inv2.corruption_case_id JOIN Organization AS o1 ON inv1.organization_id = o1.id JOIN Organization AS o2 ON inv2.organization_id = o2.id
WHERE o1.name < o2.name;` |
| List all individuals and the organizations they are involved with | `SELECT Individual.name, Organization.name FROM Individual JOIN Involvement ON Individual.id = Involvement.individual_id JOIN Organization ON Involvement.organization_id = Organization.id;` |
| List all organizations and the individuals they are involved with: | `SELECT Organization.name, Individual.name FROM Organization JOIN Involvement ON Organization.id = Involvement.organization_id JOIN Individual ON Involvement.individual_id = Individual.id;` |

