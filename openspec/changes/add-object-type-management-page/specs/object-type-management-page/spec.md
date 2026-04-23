## ADDED Requirements

### Requirement: Object type list supports keyword search
The system SHALL allow users to search object types by name keyword and refresh list results using current pagination defaults.

#### Scenario: Search by type name
- **WHEN** user inputs a keyword and triggers search
- **THEN** system requests list data filtered by the keyword and displays matching rows

### Requirement: Object type list renders required columns
The system SHALL render at least these columns for each row: type name, status, visibility, date, and action entry.

#### Scenario: Render required columns after data load
- **WHEN** list data is successfully returned
- **THEN** system renders all required columns with row-aligned values

### Requirement: Object type list supports pagination controls
The system SHALL support page number change and page size change, and SHALL refresh list data according to the latest pagination state.

#### Scenario: Change page number
- **WHEN** user selects a different page number
- **THEN** system requests and renders data for the selected page

#### Scenario: Change page size
- **WHEN** user changes page size option
- **THEN** system resets to first page and requests data using the new page size

### Requirement: Object type list provides view action
The system SHALL provide a view action for each row to open that object type in the defined detail flow.

#### Scenario: Trigger view action
- **WHEN** user clicks the view action in a row
- **THEN** system opens the detail target bound to that object type record
