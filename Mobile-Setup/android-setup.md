# Android Setup Guide

This guide will help you set up the Tiny SRS spaced repetition app on your Android device for offline use.

## Method 1: Using Chrome Browser (Recommended)

### Step 1: Download Files to Your Device
1. Open Google Drive in Chrome on your Android device
2. Navigate to your shared spaced repetition folder
3. Download the `tiny_srs_single_file_offline.html` file
4. Download your CSV deck files
5. Files will be saved to your device's Downloads folder

### Step 2: Open the App
1. Open Chrome on your Android device
2. Type `file:///` in the address bar
3. Navigate to Downloads folder
4. Tap on `tiny_srs_single_file_offline.html`
5. The app will open and work completely offline

### Step 3: Load Your Decks
1. Tap "Choose File" to select a CSV deck
2. Navigate to Downloads and select your CSV file
3. The deck will load with your personal progress
4. Study anywhere, anytime - no internet required!

## Method 2: Using File Manager + Browser

### Step 1: Download Files
1. Use Google Drive app to download files to device
2. Or use any file manager to save files locally

### Step 2: Open with Browser
1. Use any file manager app (e.g., ES File Explorer, Solid Explorer)
2. Navigate to where you saved the HTML file
3. Long-press the HTML file
4. Select "Open with" → "Chrome" (or your preferred browser)

## Method 3: Create a Desktop Shortcut

### For Frequent Use:
1. After opening the app in Chrome (Method 1)
2. Tap the three dots menu (⋮) in Chrome
3. Select "Add to Home screen"
4. Give it a name like "My Study Cards"
5. The app icon will appear on your home screen
6. Tap the icon to launch directly into the app

## Tips for Android Use

### Battery Optimization:
- The app uses minimal battery since it's just HTML/JavaScript
- No background processes or network activity
- Perfect for long study sessions

### Storage:
- The HTML file is only ~15KB
- CSV files are typically 1-50KB each
- Your progress data is stored in browser storage (very small)

### Screen Orientation:
- Works in both portrait and landscape modes
- Portrait is recommended for better readability on phones
- Landscape works well on tablets

### Offline Usage:
- Once files are downloaded, no internet connection needed
- Progress is saved locally on your device
- Sync progress by sharing localStorage backup (see Progress-Export-Examples)

## Troubleshooting

### File Access Issues:
- If file:// URLs don't work, try a different browser
- Firefox, Opera, and Samsung Internet also work well
- Some browsers may block local file access for security

### CSV Loading Problems:
- Ensure CSV files are in the same Downloads folder
- Check that CSV files have proper format (see Templates folder)
- File names with special characters may cause issues

### Progress Not Saving:
- Make sure you're using the same browser each time
- Browser storage can be cleared if you clear browsing data
- Export progress regularly (see Progress-Export-Examples)

### Multiple Users:
- Use the "Change User" button to switch between users
- Each user gets separate progress tracking
- Remember to reload your CSV deck after changing users

## Performance Notes

- Works smoothly on most Android devices from 2018+
- Older devices may have slower CSV parsing for very large decks
- Recommended deck size: 500-2000 cards for optimal performance
- Split larger decks into smaller themed decks if needed

## Security & Privacy

- All data stays on your device - nothing sent to servers
- No permissions required beyond file access
- No ads, tracking, or data collection
- Perfect for sensitive study materials