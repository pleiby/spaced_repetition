# Tiny-SRS: Offline Spaced Repetition System

A simple, privacy-first spaced repetition flashcard application built as a single HTML file. Study vocabulary, facts, or any question-answer pairs using scientifically-proven spaced repetition algorithms.

## 👥 Authors

**Paul Leiby** & **GitHub Copilot**

*Created through collaborative development: Paul provided the vision, requirements, and design decisions while GitHub Copilot implemented the technical solutions and code.*

## ✨ Features

- **🔒 Completely Offline**: No servers, no tracking, no internet required
- **📱 Responsive Design**: Works on desktop, tablet, and mobile devices
- **🧠 SM-2 Algorithm**: Scientifically-proven spaced repetition scheduling
- **📊 Progress Tracking**: Automatic progress saving in browser localStorage
- **⌨️ Keyboard Shortcuts**: Efficient study with hotkeys
- **📁 CSV Import**: Load study decks from standard CSV files
- **🎯 Multiple Study Modes**: Review due cards or cram all cards
- **⚡ Fast Mode**: Accelerated testing (1 day = 1 minute)
- **🔄 Auto-shuffle**: Randomize card order on import
- **💾 Export Progress**: Backup your study data as JSON

## 🚀 Quick Start

### For Individual Use:
1. **Download the Application**
   - Save `tiny_srs_single_file_offline.html` to your device
   - Download desired CSV files from the `CSV-Decks/` folder

2. **Open the Application**
   - Double-click the HTML file or open in any modern web browser
   - **Mobile**: Transfer to phone and open with Chrome/Firefox

3. **Load a Study Deck**
   - Click "Load CSV deck"
   - Select a CSV file with your study material
   - Example: Try `french_vocabulary_500.csv` from Languages folder

4. **Configure Settings** (optional)
   - ✅ **Fast mode**: Makes 1 day = 1 minute for testing
   - ✅ **Shuffle**: Randomizes card order
   - ⚪ **Auto-reveal**: Shows answers automatically for familiar cards

5. **Start Studying!**
   - Read the question/cue
   - Think of your answer
   - Press **Space** to reveal the correct answer
   - Grade yourself: **1** (Again), **2** (Hard), **3** (Good), **4** (Easy)

### For Shared/Group Use:
1. **Each person downloads their own copy** of the HTML file to their device
2. **Share CSV decks** through the common Google Drive folder
3. **Individual progress** is automatically maintained per device/browser
4. **Export progress regularly** for personal backup

## 📋 CSV File Format

Create study decks using standard CSV files with two columns:

```csv
cue,answer
What is the capital of France?,Paris
2 + 2 = ?,4
Bonjour,Hello
```

### Supported Column Names

The parser automatically recognizes these column headers:

**For Questions/Cues:**
- `cue`, `question`, `front`, `q`, `prompt`

**For Answers:**
- `answer`, `back`, `a`, `response`, `resp`

**No headers?** No problem! The first two columns will be used as cue and answer.

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Space** | Reveal/hide answer |
| **Enter** | Skip to next due card |
| **1** | Grade: Again (failed - start over) |
| **2** | Grade: Hard (difficult recall) |
| **3** | Grade: Good (normal recall) |
| **4** | Grade: Easy (effortless recall) |
| **s** | Suspend/unsuspend current card |

## 🧠 How Spaced Repetition Works

The application uses a modified **SM-2 algorithm** that adjusts review intervals based on your performance:

### Grading System
- **Grade 1 (Again)**: Complete failure → Reset progress, review soon
- **Grade 2 (Hard)**: Difficult recall → Shorter interval, reduced ease
- **Grade 3 (Good)**: Normal recall → Standard interval progression
- **Grade 4 (Easy)**: Effortless recall → Longer interval, increased ease

### Reverse Mode
- **Bidirectional Study**: Toggle "Reverse cards" to show answers first and test recall of questions
- **Same Statistics**: Card progress remains unchanged when switching directions - both forward and reverse test the same memory association
- **Perfect for Languages**: Test "Bonjour → Hello" and "Hello → Bonjour" using the same card data

### Scheduling Logic
1. **First review**: 1 day (or 1 minute in fast mode)
2. **Second review**: 6 days (or 6 minutes in fast mode)
3. **Subsequent reviews**: Previous interval × ease factor

### Ease Factor
- Starts at 2.5 for new cards
- Increases with easy grades (max ~3.0)
- Decreases with difficult grades (min 1.3)
- Determines how quickly intervals grow

## 📊 Study Modes

### Review Due
- Shows only cards that are currently due for review
- Optimal for regular study sessions
- Follows spaced repetition timing

### Cram All
- Shows all cards regardless of due time
- Useful for intensive review sessions
- Ignores spaced repetition scheduling

## 💾 Data Management & Progress Tracking

### How Progress Tracking Works

The application automatically tracks your learning progress across all study sessions using a sophisticated local storage system:

#### **Deck Identification**
- Each CSV file gets a unique identifier (hash) based on its content
- Hash format: `tinySRS:a1b2c3d4` (first 6 characters shown in UI)
- Same CSV content = same deck ID, preserving progress
- Modified CSV content = new deck ID, fresh start

#### **Card-Level Data Tracking**
Each flashcard stores comprehensive study statistics:

```javascript
{
  id: 0,                    // Sequential card number
  cue: "Bonjour",          // Question/prompt text
  answer: "Hello",         // Correct answer
  n: 3,                    // Number of successful reviews
  ef: 2.8,                 // Ease factor (1.3 to ~3.0)
  ivl: 172800000,          // Next interval in milliseconds
  due: 1695123456789,      // Due time (timestamp)
  lapses: 1,               // Times you got it wrong
  suspended: false         // Whether card is disabled
}
```

#### **Session Persistence**
- **Automatic saves** after every card grading
- **Instant updates** to browser localStorage
- **No manual save required** - everything is automatic
- **Works offline** - no internet connection needed

### Storage Location & Technical Details

#### **Browser localStorage**
- **Location**: Your browser's local storage database
- **Namespace**: Keys prefixed with `tinySRS:`
- **Capacity**: Typically 5-10MB per domain
- **Persistence**: Survives browser restarts, computer reboots
- **Scope**: Domain-specific (only this HTML file can access)

#### **Storage Structure**
```
localStorage['tinySRS:a1b2c3'] = {
  "name": "french_vocabulary_500.csv",
  "cards": [
    { /* card 1 data */ },
    { /* card 2 data */ },
    // ... all cards with progress
  ]
}
```

#### **Cross-Session Behavior**
1. **First Load**: New deck creates fresh cards with default values
2. **Subsequent Loads**: Existing progress merged with CSV data
3. **Card Matching**: Uses `cue + answer` combination as unique key
4. **Data Merging**: New cards added, existing cards preserve progress
5. **Automatic Cleanup**: Only cards present in CSV are kept

### Progress Preservation Scenarios

#### **✅ Progress WILL Be Saved:**
- Closing and reopening the HTML file
- Browser restart
- Computer restart
- Loading same CSV file multiple times
- Switching between different decks

#### **❌ Progress WILL Be Lost:**
- Using browser's private/incognito mode
- Clearing browser data/localStorage
- Using different browser or device
- Moving HTML file to different domain/location

#### **⚠️ Progress MIGHT Be Lost:**
- Browser storage quota exceeded
- Browser automatic cleanup (rare)
- System crashes during save operation

### Export & Backup Options

#### **Automatic Export**
Click "Export progress" to download complete study data:

```json
{
  "name": "french_vocabulary_500.csv",
  "key": "a1b2c3d4e5f6",
  "exported": "2025-09-19T10:30:00.000Z",
  "cards": [
    {
      "cue": "Bonjour",
      "answer": "Hello", 
      "n": 3,
      "ef": 2.8,
      "ivl": 172800000,
      "due": 1695123456789,
      "lapses": 1,
      "suspended": false
    }
    // ... all cards with complete statistics
  ]
}
```

#### **Manual Backup Strategy**
1. **Regular exports**: Download JSON files periodically
2. **Multiple locations**: Store backups in different folders
3. **Version control**: Date your backup files
4. **Cloud sync**: Put backups in Dropbox/Google Drive/etc.

### Data Recovery & Migration

#### **Moving Between Devices**
Since localStorage is browser-specific, to transfer progress:
1. Export progress from original device
2. Copy HTML file to new device
3. Load CSV deck on new device
4. Manually import progress (requires custom modification)

#### **Browser Changes**
- **Same browser, same device**: Progress preserved
- **Different browser**: Must export/import progress
- **Browser profile reset**: Progress lost unless exported

### Reset Progress
- "Reset ease" button clears all progress data
- Returns all cards to initial state (n=0, ef=2.5, due=now)
- **Cannot be undone** - export first if you want backup!
- Useful for starting fresh or fixing corrupted data

### Privacy & Security Implications

#### **What's Stored Locally:**
- Your study progress and statistics
- Card content (questions and answers)
- Review history and timing data
- Settings preferences

#### **What's NOT Stored:**
- Personal identifying information
- Study session times/durations
- Device or browser information
- Any data on external servers

#### **Data Access:**
- Only this HTML file can read the data
- Other websites cannot access your progress
- Data stays on your device unless you export it
- No telemetry or analytics collection

### Progress Tracking Summary

**🔄 How It Works:**
1. Load CSV → Generate unique deck ID → Check for existing progress
2. Study cards → Grade performance → Update statistics automatically
3. Algorithm calculates next review time based on your performance
4. Close/reopen anytime → Progress seamlessly continues where you left off

**💡 Key Benefits:**
- **Zero setup required** - progress tracking is completely automatic
- **Cross-session continuity** - pick up exactly where you left off
- **Performance-based scheduling** - difficult cards appear more often
- **Complete privacy** - all data stays on your device
- **Export flexibility** - backup and analyze your progress anytime

## 🔧 Advanced Features

### Fast Mode
Enable "Fast mode" to accelerate time for testing:
- 1 day becomes 1 minute
- 1 week becomes ~7 minutes
- Experience full spaced repetition in a single session

### Card Suspension
- Press 's' to suspend difficult or irrelevant cards
- Suspended cards won't appear in reviews
- Press 's' again to reactivate

### Auto-reveal
- Automatically shows answers for cards you've seen 3+ times
- Reduces clicking for familiar material
- Can be enabled in settings

## � Mobile Usage

### Android Setup
1. **Transfer HTML file to your phone:**
   - Email attachment, Google Drive, or USB transfer
   - Save to Downloads or easily accessible folder

2. **Access via browser:**
   - Open Chrome/Firefox → Navigate to file location
   - Or use file manager → Tap HTML file → "Open with Browser"

3. **Add to Home Screen (optional):**
   - In Chrome: Menu ⋮ → "Add to Home Screen"
   - Creates app-like icon for quick access

4. **Load CSV decks:**
   - Transfer CSV files to phone storage
   - Use browser's file picker to select decks

### iOS Setup
1. **Transfer files via:**
   - AirDrop, iCloud Drive, or email
   - Save to Files app

2. **Open in Safari:**
   - Files app → Tap HTML file → Share → Safari
   - Or copy to Safari's local storage

3. **Add to Home Screen:**
   - Safari → Share button → "Add to Home Screen"

## 👥 Shared Usage & Collaboration

### Individual Progress Guarantee
- **Progress is device/browser specific** - each person maintains separate study data
- **Same CSV files, individual tracking** - everyone can use shared decks with personal progress
- **No cross-contamination** - User A's progress never affects User B's study session

### Setting Up Shared Access
1. **Create shared cloud folder** (Google Drive, Dropbox, etc.)
2. **Include:**
   - Main HTML application file
   - Collection of CSV study decks
   - Setup instructions for different devices
   - Progress export examples

3. **Each user downloads:**
   - HTML file to their personal device
   - Desired CSV decks for study
   - Maintains individual progress automatically

### Best Practices for Groups
- **Download, don't stream** - Save files locally for best performance
- **Regular progress exports** - Backup personal study data
- **Organize CSV decks by subject** - Languages, Academic, Professional, etc.
- **Share new decks** - Contribute quality study materials to the group
- **Use descriptive filenames** - `spanish_verbs_beginner_300words.csv`

### Creating Study Groups
1. **Shared deck creation** - Collaborate on comprehensive CSV files
2. **Progress comparison** - Export and compare study statistics
3. **Motivation tracking** - Share milestones and achievements
4. **Deck requests** - Request specific subject areas from group members

## 📁 File Structure

```
Tiny-SRS-Spaced-Repetition/           # Shared cloud folder
├── tiny_srs_single_file_offline.html # Main application
├── README.md                         # This documentation
├── CSV-Decks/                        # Study materials
│   ├── Languages/
│   │   ├── french_vocabulary_500.csv
│   │   ├── spanish_basics_300.csv
│   │   └── german_verbs.csv
│   ├── Academic/
│   │   ├── biology_terms.csv
│   │   ├── chemistry_formulas.csv
│   │   └── history_dates.csv
│   └── Professional/
│       ├── medical_terminology.csv
│       └── programming_concepts.csv
├── Mobile-Setup/                     # Device-specific guides
│   ├── android_instructions.md
│   └── ios_instructions.md
└── Progress-Examples/                # Backup examples
    ├── sample_export.json
    └── backup_guide.md
```

## 🌟 Example Study Decks

The included `french_vocabulary_500.csv` contains:
- 500+ common French words and phrases
- Essential vocabulary for beginners
- Organized by themes (food, family, verbs, etc.)
- Perfect for testing the application

Create your own decks for:
- **Language Learning**: Vocabulary, phrases, grammar
- **Academic Study**: Facts, definitions, formulas
- **Professional Training**: Terms, procedures, concepts
- **Personal Knowledge**: Anything you want to memorize!

## 🛡️ Privacy & Security

- **No Internet Required**: Everything runs locally in your browser
- **No Data Collection**: Zero tracking, analytics, or telemetry
- **No Account Needed**: No sign-ups, passwords, or personal info
- **Browser Storage Only**: Data stays in your browser's localStorage
- **Open Source**: Single HTML file - inspect the code yourself

## 🔧 Technical Details

- **Technology**: Pure HTML5, CSS3, and vanilla JavaScript
- **Storage**: Browser localStorage (5-10MB typical limit)
- **Compatibility**: Works in all modern browsers
- **File Size**: ~50KB single file
- **Dependencies**: None - completely self-contained

## 🚨 Troubleshooting

### CSV Import Issues
- Ensure your file has `.csv` extension
- Check that columns contain data
- Verify proper comma separation
- Try with/without headers

### Progress Not Saving
- Check if localStorage is enabled
- Private/incognito mode may not persist data
- Browser storage might be full

### Performance Issues
- Large decks (1000+ cards) may be slower
- Consider splitting into smaller themed decks
- Close other browser tabs to free memory

## 📈 Study Best Practices

1. **Study Regularly**: Daily 15-30 minute sessions work best
2. **Grade Honestly**: Accurate self-assessment improves the algorithm
3. **Don't Rush**: Take time to really think before revealing answers
4. **Use Context**: Create connections between related cards
5. **Review Mistakes**: Focus extra attention on "Again" cards
6. **Stay Consistent**: Regular practice is more effective than cramming

## 🤝 Contributing

This is a single-file application designed for simplicity and portability. To modify:

1. Open `tiny_srs_single_file_offline.html` in a text editor
2. The file contains HTML, CSS, and JavaScript all in one
3. Look for detailed comments explaining each section
4. Test changes by opening in a browser

## 📄 License

This project is released into the public domain. Use, modify, and distribute freely.

## 🙏 Acknowledgments

- Based on the SM-2 spaced repetition algorithm by Piotr Wozniak
- Inspired by Anki and other spaced repetition systems
- Built for privacy-conscious learners who want local control

---

**Happy Learning!** 🎓

*Remember: Spaced repetition works best with consistent practice. Little and often beats cramming every time!*