# Progress Export and Backup Examples

This directory contains examples and scripts for backing up and restoring your spaced repetition progress data.

## Understanding Your Progress Data

Your study progress is stored in your browser's localStorage with keys like:
- `tinySRS:userId` - Your user identifier
- `tinySRS:progress_{deckKey}` - Progress data for each deck you've studied

## Example 1: Manual Progress Export

### JavaScript Console Method

Open your browser's developer console (F12) and run:

```javascript
// Export all TinySRS data
function exportTinySRSData() {
    const data = {};
    for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key.startsWith('tinySRS:')) {
            data[key] = localStorage.getItem(key);
        }
    }
    return JSON.stringify(data, null, 2);
}

// Run the export
const backup = exportTinySRSData();
console.log(backup);

// Download as file
const blob = new Blob([backup], {type: 'application/json'});
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = `tinysrs-backup-${new Date().toISOString().split('T')[0]}.json`;
a.click();
```

## Example 2: Import Progress Data

### Restore from Backup File

```javascript
// Import TinySRS data from JSON
function importTinySRSData(jsonString) {
    try {
        const data = JSON.parse(jsonString);
        let imported = 0;
        
        for (const [key, value] of Object.entries(data)) {
            if (key.startsWith('tinySRS:')) {
                localStorage.setItem(key, value);
                imported++;
            }
        }
        
        alert(`Successfully imported ${imported} items`);
        return true;
    } catch (error) {
        alert('Error importing data: ' + error.message);
        return false;
    }
}

// To use: paste your JSON backup and run:
// importTinySRSData(YOUR_JSON_STRING_HERE);
```

## Example 3: Cross-Device Sync Script

### Share Progress Between Devices

Save this as `sync-progress.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>TinySRS Progress Sync</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 20px; }
        textarea { width: 100%; height: 200px; font-family: monospace; }
        button { padding: 10px 20px; margin: 10px; font-size: 16px; }
        .section { margin: 20px 0; padding: 20px; border: 1px solid #ddd; }
    </style>
</head>
<body>
    <h1>TinySRS Progress Sync Tool</h1>
    
    <div class="section">
        <h2>Export Progress</h2>
        <p>Click to export your current progress data:</p>
        <button onclick="exportData()">Export Progress</button>
        <textarea id="exportArea" placeholder="Exported data will appear here..."></textarea>
    </div>
    
    <div class="section">
        <h2>Import Progress</h2>
        <p>Paste exported progress data and click import:</p>
        <textarea id="importArea" placeholder="Paste exported progress data here..."></textarea>
        <button onclick="importData()">Import Progress</button>
    </div>
    
    <script>
        function exportData() {
            const data = {};
            for (let i = 0; i < localStorage.length; i++) {
                const key = localStorage.key(i);
                if (key.startsWith('tinySRS:')) {
                    data[key] = localStorage.getItem(key);
                }
            }
            document. getElementById('exportArea').value = JSON.stringify(data, null, 2);
        }
        
        function importData() {
            const jsonString = document.getElementById('importArea').value;
            try {
                const data = JSON.parse(jsonString);
                let imported = 0;
                
                for (const [key, value] of Object.entries(data)) {
                    if (key.startsWith('tinySRS:')) {
                        localStorage.setItem(key, value);
                        imported++;
                    }
                }
                
                alert(`Successfully imported ${imported} items`);
            } catch (error) {
                alert('Error importing data: ' + error.message);
            }
        }
    </script>
</body>
</html>
```

## Example 4: Automatic Backup Script

### Daily Backup Automation

Create a bookmark with this JavaScript URL:

```javascript
javascript:(function(){
    const data = {};
    for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i);
        if (key.startsWith('tinySRS:')) {
            data[key] = localStorage.getItem(key);
        }
    }
    const backup = JSON.stringify(data, null, 2);
    const blob = new Blob([backup], {type: 'application/json'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'tinysrs-backup-' + new Date().toISOString().split('T')[0] + '.json';
    a.click();
})();
```

## Example Progress Data Structure

### Sample Backup File

```json
{
  "tinySRS:userId": "John",
  "tinySRS:progress_a1b2c3d4_e5f6g7h8": "{\"cards\":[{\"front\":\"Hello\",\"back\":\"Bonjour\",\"interval\":1,\"repetition\":1,\"ef\":2.5,\"due\":1727654400000},{\"front\":\"Goodbye\",\"back\":\"Au revoir\",\"interval\":4,\"repetition\":2,\"ef\":2.6,\"due\":1727913600000}],\"lastStudied\":1727568000000,\"totalCards\":2,\"studiedCards\":2}",
  "tinySRS:progress_x9y8z7w6_v5u4t3s2": "{\"cards\":[{\"front\":\"2+2\",\"back\":\"4\",\"interval\":1,\"repetition\":1,\"ef\":2.5,\"due\":1727654400000}],\"lastStudied\":1727568000000,\"totalCards\":1,\"studiedCards\":1}"
}
```

## Migration Scenarios

### Moving to New Device

1. Export progress from old device using any method above
2. Install TinySRS on new device
3. Import the progress data
4. Verify your study streaks and card intervals are preserved

### Sharing Progress with Study Group

1. Each member exports their progress
2. Combine progress files (ensure different userIds)
3. Import combined data on shared device
4. Use "Change User" feature to switch between members

### Backup Best Practices

1. **Regular Exports**: Export weekly or after significant study sessions
2. **Multiple Locations**: Save backups to cloud storage, email, USB drive
3. **Version Control**: Include date in backup filename
4. **Test Restores**: Periodically test that your backups work
5. **Group Coordination**: Establish backup schedule for shared usage

## Troubleshooting

### Common Issues

- **Empty Export**: Make sure you're running the script on the same domain where you study
- **Import Fails**: Verify JSON format is valid using a JSON validator
- **Missing Progress**: Check that localStorage wasn't cleared by browser cleanup
- **Wrong User Data**: Ensure you're importing data for the correct user ID

### Browser Differences

- **Chrome**: Best localStorage persistence, good dev tools
- **Safari**: May clear data more aggressively, export more frequently  
- **Firefox**: Good persistence, privacy settings may affect localStorage
- **Mobile Browsers**: Limited localStorage, export after each session