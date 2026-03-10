## Test plan – Step 1 booking flow

### 1. Preconditions
- Staging environment `https://app.staging.shipsticks.com` is reachable.
- Booking widget is available on the landing page for unauthenticated users.
- Test scenario data is fixed as provided in the challenge:
  - Shipment type: One-way
  - Item: 1 Golf Bag (Standard)
  - Origin: 1234 Main Street, Los Angeles, CA, USA
  - Destination: 4321 Main St, Miami Lakes, FL, USA
  - Service level: Ground
  - Delivery date: Wednesday, April 8, 2026
- Browser and Playwright for Python are installed and can run headed tests.
- No existing bookings or session state interfere with a new Step 1 flow.

### 2. Happy path steps
- Open the ShipSticks staging homepage.
- Locate the booking widget (Where from? / Where to? inputs, shipment type, date, and service level controls).
- Ensure “One-way” shipment type is selected.
- Select “1 Golf Bag (Standard)” as the item.
- Click the origin field, type `1234 Main Street, Los Angeles, CA, USA`, and choose the matching autocomplete result.
- Click the destination field, type `4321 Main St, Miami Lakes, FL, USA`, and choose the matching autocomplete result.
- Open the date picker and select Wednesday, April 8, 2026.
- Select “Ground” as the service level.
- Click “Get started” to proceed from the booking widget into Step 1.
- Verify that Step 1 shows a summary or review state with the selected item, addresses, date, and service level, and that the flow is ready to move to Step 2.

### 3. Key assertions
- Booking widget elements are visible on load:
  - Origin and destination inputs.
  - Shipment type selector (including One-way).
  - Date picker control.
  - Service level options (including Ground).
  - “Get started” button.
- “One-way” option is available, can be selected, and remains selected after interaction.
- Origin and destination inputs:
  - Accept the full addresses.
  - Do not remain empty after autocomplete selection (indicating the UI accepted the addresses).
- Date picker:
  - Allows selecting April 8, 2026.
  - Displays April 8, 2026 as the chosen delivery date.
- Service level:
  - “Ground” can be selected.
  - “Ground” appears as the chosen service level in the UI or summary.
- Step 1 completion:
  - Step 1 header or progress indicator is present and marked active/complete.
  - Summary panel (or equivalent) reflects:
    - Shipment type: One-way
    - Item: 1 Golf Bag (Standard)
    - Correct origin and destination addresses
    - Service level: Ground
    - Delivery date: April 8, 2026
  - A control to proceed to Step 2 (e.g., “Continue” / “Next”) is enabled, indicating the flow is ready to advance.

### 4. Top 3 risks
- **Address autocomplete flakiness:** Slow or inconsistent autocomplete responses may cause the wrong address to be selected or leave inputs appearing filled but not truly validated by the backend.
- **Date and service availability rules:** Pricing or scheduling logic could silently reject the requested date or Ground service (e.g., blackout dates, cut-off times), leading to confusing validation states for users.
- **DOM and locator changes:** Future UI refactors (renaming labels, changing roles, or restructuring the booking widget) may break brittle locators, especially if they rely on CSS selectors or fragile text instead of stable roles and accessible names.
