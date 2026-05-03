# Allowance Tracker Specification

## Overview
A self-contained web application to track time-based quotas (allowances) for various services. Each service has two reset cycles: a short-term 5-hour cycle and a long-term 7-day cycle.

## Core Features
- **Multi-Service Management**: Add, rename, and remove services.
- **Dual Quota Tracking**:
    - **5-Hour Cycle**: Tracking progress over a 5-hour period.
    - **7-Day Cycle**: Tracking progress over a 7-day period.
- **Persistence**: All data stored in `localStorage`.
- **Import/Export**: Export configuration to JSON and import from JSON.
- **Modern UI**:
    - Dashboard view showing progress bars and percentages.
    - Configuration mode (modal or collapsible) to set initial reset timestamps.
    - Tailwind CSS 4.x for styling.
    - Material Icons for iconography.

## Data Model
The data will be stored as a JSON object in `localStorage` under the key `allowance_tracker_data`.

```json
{
  "services": [
    {
      "id": "uuid-string",
      "name": "Service Name",
      "nextReset5h": 1714700000000, // Unix timestamp (ms) of the NEXT reset for the 5h period
      "nextReset7d": 1714700000000  // Unix timestamp (ms) of the NEXT reset for the 7d period
    }
  ]
}
```

### Calculation Logic
- **Period Duration**:
    - 5 Hours = 5 * 60 * 60 * 1000 ms = 18,000,000 ms.
    - 7 Days = 7 * 24 * 60 * 60 * 1000 ms = 604,800,000 ms.
- **Progress Calculation**:
    - `remaining = NextResetTime - CurrentTime`
    - If `remaining < 0`, the period has reset. Calculate the new `NextResetTime` by adding `PeriodDuration` until it is in the future.
    - `percentage = ( (PeriodDuration - remaining) / PeriodDuration ) * 100`
    - This reflects how much of the current quota has been "used" or how far we are into the period leading up to the end.

## UI/UX Design
- **Dashboard**: A grid or list of cards. Each card displays the service name and two progress bars.
- **Settings Modal**:
    - Fields to edit service name.
    - Date/Time pickers to set the `StartTime` for both 5h and 7d periods.
    - Button to delete the service.
- **Global Actions**:
    - "Add Service" button.
    - "Export JSON" and "Import JSON" buttons.
    - "Toggle Settings Visibility" to hide the configuration inputs once set.

## Technology Stack
- **Framework**: Vanilla JS + Tailwind CSS 4.x (CDN).
- **Icons**: Material Symbols / Icons.
- **Storage**: Browser LocalStorage.

## Implementation Steps
1. **Setup**: Create `allowance-tracker.html` with Tailwind 4.x CDN and Material Icons link.
2. **State Management**: Create a central state object and functions to save/load from `localStorage`.
3. **UI Components**:
    - Header with Global Actions.
    - Service Cards with progress tracking.
    - Settings Modal.
4. **Logic Implementation**:
    - Timer interval (every second or minute) to update progress bars.
    - Import/Export functionality.
5. **Polishing**: Add animations, transitions, and ensure responsiveness.
