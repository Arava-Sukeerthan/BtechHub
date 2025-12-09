# 🔗 Supabase Integration Example

## How to Link Your Supabase PDFs

### Step 1: Upload PDF to Supabase Storage

1. Go to your Supabase Dashboard
2. Navigate to **Storage**
3. Create a bucket called `materials` (if not exists)
4. Upload your PDF files

### Step 2: Get Public URL

1. Click on the uploaded file
2. Click **Get URL** or **Copy URL**
3. You'll get a URL like:
   ```
   https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/your-file.pdf
   ```

### Step 3: Update curriculum.js

Open `src/data/curriculum.js` and paste the URL:

```javascript
{
    id: "cse-1-math1",
    title: "Engineering Mathematics - I",
    type: "notes",
    description: "Calculus and Linear Algebra",
    url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/math1.pdf",
    price: 9
}
```

## Example: Complete Subject with Supabase URL

```javascript
cse: {
    name: "Computer Science & Engineering",
    semesters: {
        1: [
            {
                id: "cse-1-math1",
                title: "Engineering Mathematics - I (Unit 1-5)",
                type: "notes",
                description: "Complete notes covering Calculus, Matrices, and Differential Equations",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/math1-notes.pdf",
                price: 9
            },
            {
                id: "cse-1-physics-qb",
                title: "Physics Question Bank 2024",
                type: "question_bank",
                description: "Previous year questions with detailed solutions",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/physics-qb.pdf",
                price: 9
            },
            {
                id: "cse-1-chemistry-textbook",
                title: "Engineering Chemistry Textbook",
                type: "textbook",
                description: "Standard reference book by Jain & Jain",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/chemistry-book.pdf",
                price: 9
            },
            {
                id: "cse-1-c-lab",
                title: "C Programming Lab Manual",
                type: "assignment",
                description: "All 12 experiments with code and output",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/c-lab-manual.pdf",
                price: 9
            }
        ]
    }
}
```

## Organizing Your Supabase Storage

### Recommended Folder Structure:

```
materials/
├── cse/
│   ├── sem1/
│   │   ├── math1-notes.pdf
│   │   ├── physics-qb.pdf
│   │   └── chemistry-book.pdf
│   ├── sem2/
│   │   ├── math2-notes.pdf
│   │   └── ds-notes.pdf
│   └── sem3/
│       └── dbms-notes.pdf
├── ece/
│   ├── sem1/
│   │   └── signals-notes.pdf
│   └── sem2/
│       └── circuits-notes.pdf
└── eee/
    └── sem1/
        └── bee-notes.pdf
```

### Benefits:
- ✅ Easy to find files
- ✅ Organized by branch and semester
- ✅ Clear naming convention
- ✅ Scalable structure

## URL Patterns

### Pattern 1: By Branch and Semester
```
https://[PROJECT].supabase.co/storage/v1/object/public/materials/[branch]/sem[number]/[filename].pdf
```

Example:
```
https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/math1.pdf
```

### Pattern 2: Flat Structure
```
https://[PROJECT].supabase.co/storage/v1/object/public/materials/[branch]-[sem]-[subject].pdf
```

Example:
```
https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse-1-math1.pdf
```

## Making Files Public

### Important: Set Bucket to Public

1. In Supabase Dashboard → Storage
2. Click on your `materials` bucket
3. Go to **Policies**
4. Create a policy:
   - **Policy Name:** Public Access
   - **Allowed Operations:** SELECT
   - **Target Roles:** public
   - **Policy Definition:** `true`

Or use this SQL:
```sql
CREATE POLICY "Public Access"
ON storage.objects FOR SELECT
TO public
USING (bucket_id = 'materials');
```

## Testing Your URLs

Before adding to curriculum.js, test the URL:

1. Copy the Supabase URL
2. Paste in browser address bar
3. PDF should open/download
4. If it works → Add to curriculum.js
5. If not → Check bucket permissions

## Quick Update Script

If you have many files to update, you can use this pattern:

```javascript
// In curriculum.js
const SUPABASE_BASE = "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials";

// Then use template literals
{
    id: "cse-1-math1",
    title: "Engineering Mathematics - I",
    type: "notes",
    description: "Calculus notes",
    url: `${SUPABASE_BASE}/cse/sem1/math1.pdf`,
    price: 9
}
```

## Example: Real Implementation

Here's how your CSE Semester 1 might look with real Supabase URLs:

```javascript
cse: {
    name: "Computer Science & Engineering",
    semesters: {
        1: [
            {
                id: "cse-1-math1",
                title: "Engineering Mathematics - I (Complete Notes)",
                type: "notes",
                description: "Handwritten notes covering all 5 units",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/EM1-Complete-Notes.pdf",
                price: 9
            },
            {
                id: "cse-1-math1-qb",
                title: "Mathematics - I Question Bank 2024",
                type: "question_bank",
                description: "500+ solved questions from previous years",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/EM1-QB-2024.pdf",
                price: 9
            },
            {
                id: "cse-1-physics",
                title: "Engineering Physics Notes",
                type: "notes",
                description: "Complete physics notes with diagrams",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/Physics-Notes.pdf",
                price: 9
            },
            {
                id: "cse-1-c-programming",
                title: "C Programming Complete Guide",
                type: "notes",
                description: "From basics to advanced with examples",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/C-Programming-Guide.pdf",
                price: 9
            },
            {
                id: "cse-1-c-lab",
                title: "C Programming Lab Manual",
                type: "assignment",
                description: "All lab experiments with code",
                url: "https://lpefhdfcdbpxvulfrgjc.supabase.co/storage/v1/object/public/materials/cse/sem1/C-Lab-Manual.pdf",
                price: 9
            }
        ],
        2: [
            // Semester 2 subjects with Supabase URLs...
        ]
    }
}
```

## Checklist

Before going live, verify:

- [ ] All PDFs uploaded to Supabase Storage
- [ ] Bucket is set to public
- [ ] URLs are tested in browser
- [ ] URLs updated in curriculum.js
- [ ] File names are descriptive
- [ ] Folder structure is organized
- [ ] All subjects have valid URLs
- [ ] No broken links

## Tips

1. **Use descriptive filenames:** `EM1-Complete-Notes.pdf` instead of `file1.pdf`
2. **Keep consistent naming:** Use same pattern for all files
3. **Test before deploying:** Open each URL in browser
4. **Backup your files:** Keep local copies
5. **Version your PDFs:** Add year/version if needed (e.g., `Math-2024.pdf`)

---

**🎉 Your Supabase integration is ready! Just upload PDFs and update the URLs in curriculum.js**
