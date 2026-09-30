# Darshan Card Specification

## Purpose

Lets users generate a personalized, shareable darshan summary card styled as a traditional invitation card, showing an ornate list of all visited mandals with clickable links, compact achievement chips for visited mandal categories, and a canvas-rendered share image that is visually identical to the on-screen card.

## Requirements

### Requirement: User can view their darshan summary card with visited mandal list
The system SHALL render a "My Darshan" card showing: (a) a compact, beautiful list of all visited mandal names as clickable links that open mandal details, (b) the total mandals visited out of 40, and (c) achievement chips for categories of visited mandals. The card SHALL be styled as a traditional invitation card with decorative borders, rangoli-styled mandal items, toran dividers, and an Indian color palette.

#### Scenario: Card renders from stored progress with mandal list
- **WHEN** user opens the "My Darshan" panel
- **THEN** system reads `localStorage.gd_visited` and `window.MANDALS`, and the card displays a compact list of every visited mandal name as a clickable link, the visited count, and achievement chips for visited categories

#### Scenario: Clicking a mandal link opens its details
- **WHEN** user clicks a visited mandal name in the card
- **THEN** the app opens the mandal's details panel

#### Scenario: No mandals visited yet
- **WHEN** user opens the panel with an empty `gd_visited` object
- **THEN** card shows "0 / 40 mandals visited" and an encouraging prompt to start darshan, with no mandal list and no chips

### Requirement: Card shows achievement chips for visited mandal categories
The system SHALL render achievement chips showing the unique category tags of all visited mandals (from `window.MANDALS[].category`). Each unique category present among visited mandals SHALL appear as a compact saffron-gradient chip. The system SHALL NOT display a separate "Completion Badges" section (earned or locked category badges).

#### Scenario: Card renders achievement chips from visited mandal categories
- **WHEN** user opens the "My Darshan" panel with some mandals visited
- **THEN** the card displays a unique achievement chip for each category that has at least one visited mandal (e.g., "Most Famous", "Heritage & Oldest", "Suburban Favourite")

#### Scenario: Multiple mandals share a category
- **WHEN** multiple visited mandals belong to the same category (e.g., three "Most Famous" mandals)
- **THEN** only one chip for that category is shown (deduplicated)

#### Scenario: No mandals visited
- **WHEN** user opens the panel with an empty `gd_visited` object
- **THEN** card shows no achievement chips

#### Scenario: Chips are compact
- **WHEN** the card displays achievement chips
- **THEN** each chip uses a font size no larger than 0.6rem and padding no larger than 0.2rem 0.5rem

### Requirement: Card does not display a "Favorite" mandal
The system SHALL NOT display a "Favorite" stat, "Favorite" label, or any favorite mandal selection on the darshan card or in the canvas-rendered share image.

#### Scenario: No favorite stat exists
- **WHEN** user opens the "My Darshan" panel
- **THEN** the card does not contain any "Favorite" label, "Favorite" stat row, or reference to a favorite mandal

### Requirement: Card is generated as a downloadable shareable image identical to the on-screen card
The system SHALL render the darshan card as a client-side canvas image that is visually identical to the on-screen card UI, including the invitation-card frame, decorative borders, rangoli-styled mandal items, toran dividers, Indian color palette, and achievement chips. The canvas height SHALL be dynamic, calculated from the actual content rather than a fixed value.

#### Scenario: User triggers share with few mandals visited
- **WHEN** user taps "Share" with 1–5 mandals visited
- **THEN** the canvas height is compact (shorter than the maximum) and all content fits without clipping

#### Scenario: User triggers share with many mandals visited
- **WHEN** user taps "Share" with 20+ mandals visited
- **THEN** the canvas height expands to fit all mandal names and achievement chips without overflow or clipping

#### Scenario: Canvas matches on-screen card
- **WHEN** user compares the on-screen card to the downloaded image
- **THEN** both show the same sections: title, subtitle, mandal count, visited mandals list, and achievement chips

### Requirement: Card state persists across sessions
The system SHALL store the darshan card's preferred state in `localStorage.gd_darshanCard` so it restores when the user returns.

#### Scenario: User returns after leaving
- **WHEN** user reopens the app and opens "My Darshan"
- **THEN** system restores the persisted card state from `localStorage.gd_darshanCard`
