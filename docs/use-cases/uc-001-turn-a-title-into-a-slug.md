### UC-001 · Turn a title into a slug

- Zone: core
- Actor: a developer who builds page addresses from post titles
- Covers: REQ-001-1; REQ-001-2
- Precondition: the developer has a post title as a string
- Scenario: the developer calls `slugify(title)` and puts the result into the page address
- Result: the address holds only lower-case letters, digits and single hyphens
- Failure guarantee: a title with no letters or digits gives an empty string, never an error
