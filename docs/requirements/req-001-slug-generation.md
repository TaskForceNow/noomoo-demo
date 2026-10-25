# Requirements · Slug generation

**REQ-001-1.** `slugify(title)` returns the title in lower case, with every run of characters other than letters and digits replaced by a single hyphen and no hyphen at either end.

**REQ-001-2.** `slugify(title)` returns an empty string for a title with no letters or digits.
