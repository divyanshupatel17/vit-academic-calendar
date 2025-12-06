# VIT Academic Calendar

Community-maintained academic calendar data for VIT (Vellore Institute of Technology), automatically extracted from VTOP using the VTOP Calendar Extension.

## 📁 Repository Structure

```
vit-academic-calendar/
├── README.md
└── calendars/
    ├── Winter_Semester_2025-26_-_CHN/
    │   ├── Winter_Semester_2025-26_-_CHN_General_Semester__DEC_JAN_FEB_MAR_APR_UpdatedDec7.json
    │   ├── Winter_Semester_2025-26_-_CHN_General_Flexible__DEC_JAN_FEB_MAR_APR_UpdatedDec7.json
    │   └── ...
    ├── Fall_Semester_2025-26_-_CHN/
    │   └── ...
    └── Summer_Semester_2024-25_-_CHN/
        └── ...
```

### Folder Organization
- Each semester has its own folder
- Folder name format: `{Semester_Name}`
- Files keep their original names with update dates

## 📄 JSON File Format

Each calendar file contains structured event data:

```json
{
  "lastUpdated": "December 7, 2025, 02:30:45 PM",
  "lastUpdatedISO": "2025-12-07T09:00:45.123Z",
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
              },
              {
                "text": "Holiday",
                "category": "General",
                "description": "Christmas"
              }
            ]
          }
        ]
      }
    },
    "JAN-2026": {
      "date": "01-JAN-2026",
      "events": {
        "days": []
      }
    }
  }
}
```

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `lastUpdated` | string | Human-readable timestamp of last update |
| `lastUpdatedISO` | string | ISO 8601 timestamp for programmatic use |
| `semester` | string | Full semester name (e.g., "Winter Semester 2025-26 - CHN") |
| `classGroup` | string | Class group name (e.g., "General (Semester)", "General (Flexible)") |
| `months` | object | Calendar data organized by month |
| `months.{MONTH}` | object | Month-specific data |
| `months.{MONTH}.date` | string | Month start date |
| `months.{MONTH}.events` | object | Events container |
| `months.{MONTH}.events.days` | array | Array of days with events |
| `days[].date` | number | Day of the month (1-31) |
| `days[].events` | array | Array of events for that day |
| `events[].text` | string | Event type (e.g., "Instructional Day", "Holiday") |
| `events[].category` | string | Event category (e.g., "General", "CAT", "Exam") |
| `events[].description` | string | Detailed event description |

## 🤝 How to Contribute

### Prerequisites
- Install the [VTOP Calendar Extension](link-to-extension)
- Have access to VTOP (VIT student/faculty)

### Method 1: Web-Based PR (Recommended)

**No GitHub token needed!**

1. **Extract Calendar Data**
   - Open VTOP and log in
   - Open the VTOP Calendar Extension
   - Select your semester and class groups
   - Click "Extract Calendar"

2. **Create Pull Request**
   - Click "GitHub PR" button on the generated file
   - Select "Web-based PR (Recommended)"
   - Click "Continue"
   - The extension will download the JSON file

3. **Follow Instructions**
   - Fork this repository (if you haven't already)
   - Navigate to the appropriate semester folder
   - Upload the downloaded JSON file
   - Create a pull request

### Method 2: API-Based PR (Advanced)

**Requires GitHub Personal Access Token**

1. **Create GitHub Token**
   - Go to [GitHub Settings > Tokens](https://github.com/settings/tokens/new?scopes=repo&description=VTOP+Calendar+Extension)
   - Select scope: **repo** (Full control of private repositories)
   - Generate and copy the token

2. **Configure Extension**
   - Click "GitHub PR" button
   - Select "API-based PR (Advanced)"
   - Enter your GitHub token
   - Click "Continue"

3. **Automatic PR Creation**
   - The extension will automatically:
     - Check for changes
     - Create a branch
     - Upload the file
     - Create a pull request

## 📋 Pull Request Guidelines

### PR Title Format
```
Update: {Semester} - {Class Group}
```
or
```
Add: {Semester} - {Class Group}
```

### PR Description Should Include
- Semester name
- Class group
- Last updated timestamp
- Total event days
- Months included
- File location

### Before Submitting
- ✅ Ensure JSON is valid
- ✅ Check that semester and class group match the file content
- ✅ Verify the file is in the correct folder
- ✅ Make sure the data is up-to-date

## 🔍 Data Validation

All pull requests are automatically validated for:
- Valid JSON format
- Required fields present
- Correct file structure
- No duplicate entries

## 📊 Using the Data

### JavaScript Example
```javascript
// Fetch calendar data
const response = await fetch('https://raw.githubusercontent.com/divyanshupatel17/vit-academic-calendar/main/calendars/Winter_Semester_2025-26_-_CHN/Winter_Semester_2025-26_-_CHN_General_Semester__DEC_JAN_FEB_MAR_APR_UpdatedDec7.json');
const calendar = await response.json();

// Get all holidays
const holidays = [];
Object.values(calendar.months).forEach(month => {
  month.events.days.forEach(day => {
    day.events.forEach(event => {
      if (event.text.includes('Holiday')) {
        holidays.push({
          date: day.date,
          month: month.date,
          description: event.description
        });
      }
    });
  });
});

console.log('Holidays:', holidays);
```

### Python Example
```python
import requests
import json

# Fetch calendar data
url = 'https://raw.githubusercontent.com/divyanshupatel17/vit-academic-calendar/main/calendars/Winter_Semester_2025-26_-_CHN/Winter_Semester_2025-26_-_CHN_General_Semester__DEC_JAN_FEB_MAR_APR_UpdatedDec7.json'
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
                    'month': month_name,
                    'description': event['description']
                })

print(f"Total instructional days: {len(instructional_days)}")
```

## 🛠️ Troubleshooting

### "File already exists"
- The calendar for this semester/class group is already up-to-date
- Check if there are actual changes before creating a PR

### "Invalid JSON"
- Re-extract the calendar from VTOP
- Ensure the file wasn't manually edited

### "Wrong folder structure"
- Files should be in `calendars/{Semester_Name}/` folder
- Keep the original filename with update date


**Last Updated:** December 2025  
