## ADDED Requirements

### Requirement: Management shell provides consistent frame
The system SHALL provide a reusable management shell layout that contains left navigation, top breadcrumb area, and a routed content container.

#### Scenario: Render shell on management route
- **WHEN** user enters any route under management module
- **THEN** system renders left navigation, top breadcrumb, and current page content in one consistent frame

#### Scenario: Preserve shared frame while switching page
- **WHEN** user navigates between management child routes
- **THEN** system keeps left navigation and top breadcrumb mounted while only updating the content area

### Requirement: Shell navigation and breadcrumb stay in sync with route
The system SHALL keep active navigation item and breadcrumb path synchronized with current route state.

#### Scenario: Active menu reflects current route
- **WHEN** current route matches a navigation item
- **THEN** system marks that item as active in the left navigation

#### Scenario: Breadcrumb updates by route metadata
- **WHEN** user enters a child page that defines breadcrumb metadata
- **THEN** system renders breadcrumb items in route hierarchy order
