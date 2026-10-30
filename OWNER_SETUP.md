# Owner sign-in setup (about 5 minutes)

The "🔒 Sign in" link on datasolutions365.com signs you in with Google, using the
same Firebase project as the snake game (`snake-game`). After you sign in, a private
Owner area shows your snake stats and quick links.

Who counts as the owner is set in the Firestore rules (step 3). Your email lives
only there, in the Firebase console, never in this public website code.

## 1. Turn on Google sign-in
1. Go to https://console.firebase.google.com and open the **snake-game** project.
2. Left menu: **Build → Authentication** → **Get started**.
3. **Sign-in method** tab → **Google** → switch **Enable** on → pick your support email → **Save**.

## 2. Allow your website to use it
1. Still in Authentication, open the **Settings** tab → **Authorized domains**.
2. Click **Add domain** → `datasolutions365.com` → **Add**.
3. Add `www.datasolutions365.com` the same way.

## 3. Lock the stats so only you can read them
1. Left menu: **Build → Firestore Database** → **Rules** tab.
2. Just under the line `match /databases/{database}/documents {`, add:

```
    // Only the owner's Google account. Put your own email here.
    function isOwner() {
      return request.auth != null
        && request.auth.token.email_verified == true
        && request.auth.token.email == 'YOUR-EMAIL@gmail.com';
    }
```

3. In the `match /stats/{day}` block, change `allow read: if true;` to `allow read: if isOwner();`
4. In the `match /errors/{id}` block, do the same: `allow read: if isOwner();`
5. Click **Publish**.

Leave the `scores_...` blocks as they are, so everyone can still see the leaderboards.
The game can still *count* visits and games; only *reading* the numbers is locked.

Note: after step 3, https://snake.datasolutions365.com/stats.html will say it couldn't
load the stats, because that page doesn't sign in. Use the Owner area on
datasolutions365.com instead, or ask Claude to add the same sign-in to stats.html.
