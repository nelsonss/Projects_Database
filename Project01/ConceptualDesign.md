# Conceptual Design

The conceptual design of the system is based on a data model that captures the key entities involved in corruption cases and the relationships between them. The data model is designed to be flexible and extensible, capable of handling a wide range of corruption cases with varying levels of complexity and detail.

## Data Model

The data model includes the following entities:

1. **Case:** This represents a specific instance of corruption. Attributes include the case ID, the date of the case, a brief description of the case, and any other relevant details.

2. **Actor:** This represents an individual or organization involved in a corruption case. Attributes include the actor ID, the name of the actor, the role of the actor in the case (e.g., perpetrator, victim, investigator), and any other relevant details.

3. **Location:** This represents the location where a corruption case occurred. Attributes include the location ID, the name of the location, the geographical coordinates of the location, and any other relevant details.

4. **Action:** This represents a specific action taken by an actor in a corruption case. Attributes include the action ID, the type of action (e.g., bribe, fraud, embezzlement), the date of the action, and any other relevant details.

The data model also includes the following relationships:

1. **Involved In:** This relationship connects actors to cases. It indicates that an actor was involved in a specific case.

2. **Occurs At:** This relationship connects cases to locations. It indicates that a case occurred at a specific location.

3. **Performs:** This relationship connects actors to actions. It indicates that an actor performed a specific action in a corruption case.

## Design Assumptions and Decisions

The design of the data model is based on several key assumptions and decisions:

1. **Flexibility:** The data model is designed to be flexible, capable of handling a wide range of corruption cases with varying levels of complexity and detail.

2. **Extensibility:** The data model is designed to be extensible, allowing for the addition of new entities and relationships as needed to accommodate new types of corruption cases or new aspects of existing cases.

3. **Normalization:** The data model is designed to be in third normal form (3NF) to eliminate redundancy and ensure data integrity.

4. **Security:** The data model includes provisions for data security, with access controls and encryption methods in place to protect sensitive data.

5. **Scalability:** The data model is designed to be scalable, capable of handling a large number of corruption cases without a significant loss of performance.
