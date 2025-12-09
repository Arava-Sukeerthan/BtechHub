# BTechHub Curriculum Management Guide

## 📚 Overview

Your BTechHub website now has a **centralized curriculum system** where each branch (CSE, ECE, EEE, MECH, CIVIL, IT) has unique subjects for all 8 semesters.

## 🎯 What Was Fixed

### Before:
- ❌ All branches showed the same subjects
- ❌ All semesters showed the same materials
- ❌ Subjects were hardcoded in multiple places
- ❌ Difficult to update or add new content

### After:
- ✅ Each branch has its own unique subjects
- ✅ Each semester within each branch has different materials
- ✅ All subjects are in ONE central file
- ✅ Easy to update by editing a single file
- ✅ Support for Supabase PDF URLs

## 📁 File Structure

```
src/
├── data/
│   └── curriculum.js          ← EDIT THIS FILE to update subjects
├── services/
│   └── materialService.js     ← Automatically loads from curriculum.js
└── pages/
    ├── MaterialsList.jsx      ← Displays subjects (no changes needed)
    └── BranchSemesters.jsx    ← Shows semesters (no changes needed)
```

## 🔧 How to Update Subjects

### To Add/Edit Subjects for Any Branch/Semester:

1. **Open** `src/data/curriculum.js`
2. **Find** the branch you want to edit (e.g., `cse`, `ece`, `mech`)
3. **Find** the semester number (e.g., `1`, `2`, `3`, etc.)
4. **Add or modify** the subject objects

### Subject Object Format:

```javascript
{
    id: "unique-id-here",              // Must be unique across all subjects
    title: "Subject Name",             // Display name
    type: "notes",                     // Type: notes, question_bank, textbook, assignment
    description: "Brief description",  // Short description
    url: "https://your-pdf-url.pdf",  // Supabase or any PDF URL
    price: 9                           // Price in rupees
}
```

### Example: Adding a New Subject to CSE Semester 1

```javascript
cse: {
    name: "Computer Science & Engineering",
    semesters: {
        1: [
            // Existing subjects...
            {
                id: "cse-1-newsubject",
                title: "Introduction to AI",
                type: "notes",
                description: "Basics of Artificial Intelligence",
                url: "https://your-supabase-url.com/ai-notes.pdf",
                price: 9
            }
        ],
        // Other semesters...
    }
}
```

## 📋 Material Types

Use these values for the `type` field:

| Type | Display Name | Icon |
|------|-------------|------|
| `notes` | Notes | 📄 |
| `question_bank` | Q-Bank | ❓ |
| `textbook` | Textbooks | 📚 |
| `assignment` | C PROGRAMMING LAB | 📝 |

## 🌐 How It Works

### User Navigation Flow:

1. **Home Page** → User selects a branch (e.g., CSE)
2. **Branch Page** → User selects a semester (e.g., Semester 1)
3. **Materials Page** → Shows subjects from `curriculum.cse.semesters[1]`
4. **PDF Viewer** → Shows the PDF when user clicks View/Download

### Behind the Scenes:

```javascript
// When user navigates to /branch/cse/sem/1
getMaterials("cse", "1")
  ↓
getMaterialsByBranchAndSemester("cse", "1")
  ↓
Returns curriculum.cse.semesters[1]
  ↓
Displays subjects on MaterialsList page
```

## 🔗 Supabase Integration

To link your Supabase PDFs:

1. Upload PDFs to Supabase Storage
2. Get the public URL
3. Update the `url` field in `curriculum.js`

Example:
```javascript
url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/your-file.pdf"
```

## 🎨 UI Features Preserved

All your existing UI features still work:
- ✅ Premium banner for unpaid users
- ✅ Lock icons on materials
- ✅ Filter tabs (All, Notes, Q-Bank, Textbooks, C PROGRAMMING LAB)
- ✅ View and Download buttons
- ✅ Payment modal
- ✅ Responsive design
- ✅ Animations and transitions

## 🚀 Adding a New Branch

To add a completely new branch (e.g., "AI & ML"):

1. Open `src/data/curriculum.js`
2. Add a new branch object:

```javascript
export const curriculum = {
    // Existing branches...
    
    aiml: {
        name: "Artificial Intelligence & Machine Learning",
        semesters: {
            1: [
                {
                    id: "aiml-1-python",
                    title: "Python Programming",
                    type: "notes",
                    description: "Python fundamentals",
                    url: "https://your-url.pdf",
                    price: 9
                }
                // Add more subjects...
            ],
            2: [
                // Semester 2 subjects...
            ],
            // Add all 8 semesters...
        }
    }
};
```

3. Update your Home page to include the new branch card

## 📊 Current Branches & Semesters

| Branch | Code | Semesters |
|--------|------|-----------|
| Computer Science | `cse` | 1-8 ✅ |
| Electronics & Communication | `ece` | 1-8 ✅ |
| Electrical & Electronics | `eee` | 1-8 ✅ |
| Mechanical | `mech` | 1-8 ✅ |
| Civil | `civil` | 1-8 ✅ |
| Information Technology | `it` | 1-8 ✅ |

## 🐛 Troubleshooting

### Problem: Subjects not showing up
**Solution:** Check that:
- Branch ID matches exactly (lowercase: `cse`, not `CSE`)
- Semester number is correct (1-8)
- Each subject has a unique `id`

### Problem: Wrong subjects appearing
**Solution:** 
- Clear browser cache
- Check the branch and semester in the URL
- Verify `curriculum.js` has correct data

### Problem: PDF not loading
**Solution:**
- Verify the URL is publicly accessible
- Check Supabase Storage permissions
- Test the URL directly in browser

## 💡 Tips

1. **Unique IDs**: Always use unique IDs like `branchcode-semester-subjectname`
   - Good: `cse-1-math1`, `ece-2-signals`
   - Bad: `1`, `math`, `subject1`

2. **Consistent Naming**: Keep branch codes lowercase in URLs
   - Routes use: `/branch/cse/sem/1`
   - Curriculum uses: `curriculum.cse.semesters[1]`

3. **Price**: Currently set to ₹9 for all materials. Change individually if needed.

4. **Descriptions**: Keep them concise (1-2 lines) for better UI display

## 📝 Quick Reference

### To update a subject:
1. Open `src/data/curriculum.js`
2. Find: `curriculum.[branch].semesters.[number]`
3. Edit the subject object
4. Save (auto-reload via Vite HMR)

### To add a subject:
1. Copy an existing subject object
2. Change `id`, `title`, `description`, `url`
3. Add to the appropriate semester array
4. Save

### To remove a subject:
1. Find the subject in `curriculum.js`
2. Delete the entire object
3. Save

## 🎉 You're All Set!

Your curriculum system is now:
- ✅ Centralized
- ✅ Easy to maintain
- ✅ Scalable
- ✅ Branch-specific
- ✅ Semester-specific
- ✅ Ready for Supabase integration

**Need to update subjects?** Just edit `src/data/curriculum.js` and save!
