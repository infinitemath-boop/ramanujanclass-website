# Connect the Teacher corner to Firebase

Do these once in the Firebase console (https://console.firebase.google.com), in the project that runs app.ramanujanclass.org.

## 1. Web settings (already filled in)
firebase-config.js already holds the settings for project ramanujan-class1 and the teacher email.

## 2. Allow the new website to use Google sign-in
Authentication → Settings → Authorized domains → Add domain:
- ramanujanclass.org
- www.ramanujanclass.org


## 3. Firestore rules (add, don't replace)
Firestore Database → Rules. Keep your class app's existing rules and add these blocks inside
`match /databases/{database}/documents { ... }`. Replace the email with yours.

```
    function isSiteTeacher() {
      return request.auth != null
        && request.auth.token.email_verified == true
        && request.auth.token.email in ['infinitemath@ramanujanclass.org'];
    }
    match /website_videos/{id}    { allow read: if true; allow write: if isSiteTeacher(); }
    match /website_materials/{id} { allow read: if true; allow write: if isSiteTeacher(); }
    match /website_questions/{id} { allow read: if true; allow write: if isSiteTeacher(); }
    match /website_settings/{id}  { allow read: if true; allow write: if isSiteTeacher(); }
    // Demo-class requests: anyone can send one, only the teacher can read them.
    match /website_enquiries/{id} {
      allow create: if request.resource.data.keys().hasOnly(['name', 'phone', 'exam', 'createdAt'])
        && request.resource.data.name is string && request.resource.data.name.size() < 100
        && request.resource.data.phone is string && request.resource.data.phone.size() < 20;
      allow read, update, delete: if isSiteTeacher();
    }
```

## 4. Storage rules (only if you want to upload files)
Storage → Rules. Add inside `match /b/{bucket}/o { ... }`:

```
    match /website/{file=**} {
      allow read: if true;
      allow write: if request.auth != null
        && request.auth.token.email_verified == true
        && request.auth.token.email in ['infinitemath@ramanujanclass.org']
        && (request.resource == null || request.resource.size < 20 * 1024 * 1024);
    }
```

If Storage is not switched on yet, Firebase may ask you to move to the pay-as-you-go Blaze plan.
Light use stays inside the free allowance. If you prefer not to, skip this step and paste
YouTube links for videos and Google Drive links for notes instead.

## 5. Try it
Once the site is live, open it, click "Teacher sign in" at the bottom,
and sign in with your teacher Gmail.

Demo-class requests appear in Firestore Database → Data → website_enquiries.
