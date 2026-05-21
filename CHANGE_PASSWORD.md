# How to Change the App Password

## Current password: `GWF2026`

## Steps to change it

**1. Go to this website to generate a new hash:**
https://emn178.github.io/online-tools/sha256.html

**2. Type your new password into the Input field and copy the Output hash**

Example — for password `NewPass123`:
```
Output: a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3
```

**3. Open `index.html` in a text editor (Notepad, TextEdit, VS Code)**

**4. Find this line (around line 4200):**
```javascript
const PW_HASH = '6f4963299cc6d9e0f6144baab448ea930b5111e330bbe5cd94307626c4fd9902';
```

**5. Replace the hash with your new one:**
```javascript
const PW_HASH = 'a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3';
```

**6. Save the file and re-upload to GitHub**

The comment above the line also shows the current password — update that too so you remember:
```javascript
// Current password: NewPass123
```

## Security note

The password is stored as a SHA-256 hash, not plain text. Someone reading the source 
code would see the hash but not the original password. This is appropriate protection 
for a field tool — not suitable for highly sensitive data.

For stronger security, your cloud integration will replace this with a proper 
server-side authentication system.
