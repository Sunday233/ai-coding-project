## ADDED Requirements

### Requirement: Status and visibility labels follow unified mapping
The system SHALL render status and visibility values using predefined label and style mappings to ensure consistent semantics across rows.

#### Scenario: Render active and visible labels
- **WHEN** a row has status "active" and visibility "visible"
- **THEN** system renders the mapped status label and visibility label with corresponding styles

#### Scenario: Render non-visible label
- **WHEN** a row has visibility "hidden"
- **THEN** system renders the mapped non-visible label and style

### Requirement: List interaction handles loading, empty, and error states
The system SHALL display explicit loading, empty, and error states during list data lifecycle.

#### Scenario: Loading state
- **WHEN** list request is in progress
- **THEN** system shows loading indicator in list region

#### Scenario: Empty state
- **WHEN** list request succeeds with zero rows
- **THEN** system shows empty-state feedback and keeps table header context clear

#### Scenario: Error state
- **WHEN** list request fails
- **THEN** system shows an error feedback message and allows user to retry

### Requirement: Row selection behavior is predictable
The system SHALL support row checkbox selection and SHALL clear selection when page changes unless cross-page selection is explicitly enabled.

#### Scenario: Select rows in current page
- **WHEN** user checks one or more row checkboxes
- **THEN** system stores selected row identifiers for current page context

#### Scenario: Selection reset on pagination
- **WHEN** user changes page number
- **THEN** system clears current selection before rendering the new page rows
