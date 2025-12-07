# VIT Academic Calendar

Community-maintained academic calendar data for VIT (Vellore Institute of Technology), automatically extracted from VTOP.

## Repository Structure

```
calendars/
├── Winter_Semester_2025-26_-_CHN/
│   ├── [Calendar_Files].json
├── Fall_Semester_2025-26_-_CHN/
│   └── [Calendar_Files].json
└── Summer_Semester_2024-25_-_CHN/
    └── [Calendar_Files].json
```

## JSON File Format

Each calendar file contains structured event data:

```json
{
  "lastUpdated": "December 7, 2025 at 10:10:34 AM",
  "lastUpdatedISO": "2025-12-07T04:40:34.625Z",
  "semester": "Winter Semester 2025-26 - CHN",
  "classGroup": "General (Semester)",
  "months": {
    "DEC-2025": {
      "date": "01-DEC-2025",
      "events": {
        "days": [
          {
            "date": 4,
            "events": [
              {
                "text": "Instructional Day",
                "category": "General",
                "description": "FID/ Working Day / Add & Drop"
              }
            ]
          }
        ]
      }
    }
  }
}
```

## Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `lastUpdated` | string | Human-readable timestamp |
| `lastUpdatedISO` | string | ISO 8601 timestamp |
| `semester` | string | Full semester name |
| `classGroup` | string | Class group name |
| `months` | object | Calendar data by month |

## How to Contribute

1. Install the VTOP Calendar Extension
2. Extract calendar data from VTOP
3. Click "GitHub PR" to create a pull request
4. Wait for automated analysis
5. Repository maintainer will review and merge

## Using the Data

### JavaScript Example

```javascript
const response = await fetch('https://raw.githubusercontent.com/divyanshupatel17/vit-academic-calendar/main/calendars/[semester]/[file].json');
const calendar = await response.json();

// Get all holidays
const holidays = [];
Object.values(calendar.months).forEach(month => {
  month.events.days.forEach(day => {
    day.events.forEach(event => {
      if (event.text.includes('Holiday')) {
        holidays.push({
          date: day.date,
          description: event.description
        });
      }
    });
  });
});
```

### Python Example

```python
import requests

url = 'https://raw.githubusercontent.com/divyanshupatel17/vit-academic-calendar/main/calendars/[semester]/[file].json'
response = requests.get(url)
calendar = response.json()

# Get all instructional days
instructional_days = []
for month_name, month_data in calendar['months'].items():
    for day in month_data['events']['days']:
        for event in day['events']:
            if 'Instructional' in event['text']:
                instructional_days.append({
                    'date': day['date'],
                    'month': month_name
                })
```

## Automated Workflows

### PR Analysis
When you create a pull request, an automated workflow will:
- Validate JSON structure
- Check field order
- Count events by month
- Compare with existing files
- Provide detailed analysis report

### README Update
When changes are merged to main:
- README is automatically updated
- Calendar list is regenerated
- Timestamps are updated

## License

This data is extracted from VTOP and is intended for educational purposes only.
