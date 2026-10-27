# Updating ramanujanclass.org

The easy way: open the site, click "Teacher sign in" at the bottom, and use the Teacher corner
to add videos, study materials, quiz questions and your photo. Changes appear at once.

The site also reads a few fixed items from `content.json`. To change those on github.com:
1. Upload any new file into the `uploads` folder (Add file → Upload files).
2. Edit `content.json` (pencil icon) and add a line for it, for example
   `{ "title": "Group theory notes", "course": "IIT JAM", "asset": "uploads/group-notes.pdf", "type": "application/pdf" }`
3. Commit. GitHub Pages republishes the site within a minute or two.

Quiz `answer` counts from 0: 0 = A, 1 = B, 2 = C, 3 = D.
