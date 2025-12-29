# Java Conversion Status

## ✅ Converted to Java
- **13_Sorting_Algorithms.md** - All sorting algorithms with complete Java implementations

## ✅ Advanced Files Created (All in Java)
- **24_Union_Find_Disjoint_Set.md** - Complete Java implementation
- **25_AVL_Tree.md** - Complete Java implementation
- **26_Segment_Tree.md** - Complete Java implementation
- **27_Fenwick_Tree_BIT.md** - Complete Java implementation
- **28_Advanced_Graph_Algorithms.md** - Complete Java implementation
- **29_Manacher_Algorithm.md** - Complete Java implementation
- **30_Advanced_DP_Patterns.md** - Complete Java implementation

## 📝 To Be Converted (Currently Python)
- 01-12: Data Structure files (Arrays, Linked Lists, Stacks, Queues, Trees, BST, Graphs, Hash Tables, Heaps, Tries, Sets, Maps)
- 14-23: Algorithm files (Searching, DP, Greedy, Backtracking, Divide & Conquer, Two Pointers, Sliding Window, String Algorithms, Recursion, Bit Manipulation)

## 🔄 Conversion Process
Each file will be systematically converted from Python to Java with:
- Complete Java class implementations
- Proper Java syntax and conventions
- Full method signatures
- Java collections (ArrayList, HashMap, PriorityQueue, etc.)
- Proper error handling where applicable

## 📌 Note
All new advanced files (24-30) are already in Java. The remaining files can be converted upon request or systematically.

# Google OAuth Setup Guide

## Quick Setup Steps

### 1. Access Google Cloud Console
- Visit: https://console.cloud.google.com/
- Sign in with your Google account

### 2. Create or Select a Project
- Click on the project dropdown at the top
- Create a new project or select an existing one

### 3. Enable Required APIs
1. Go to **APIs & Services** → **Library**
2. Search for **"Google+ API"** or **"Google Identity Services API"**
3. Click on it and press **Enable**

### 4. Configure OAuth Consent Screen
1. Go to **APIs & Services** → **OAuth consent screen**
2. Select **External** (unless you have Google Workspace)
3. Fill in required fields:
    - **App name**: Your app name
    - **User support email**: Your email
    - **Developer contact information**: Your email
4. Click **Save and Continue**
5. On **Scopes** page, click **Save and Continue**
6. On **Test users** page (if External), add test users or skip
7. Click **Back to Dashboard**

### 5. Create OAuth 2.0 Client ID
1. Go to **APIs & Services** → **Credentials**
2. Click **+ CREATE CREDENTIALS** → **OAuth 2.0 Client ID**
3. If prompted, select **Web application**
4. Fill in:
    - **Name**: "Google OAuth App" (or any name)
    - **Authorized JavaScript origins**:
        - `http://localhost:3000` (for frontend)
    - **Authorized redirect URIs**:
        - `http://localhost:8080/login/oauth2/code/google` ⚠️ **THIS IS CRITICAL**
5. Click **Create**
6. **Copy the Client ID and Client Secret** (you'll need these)

### 6. Update Application Configuration

Edit `backend/src/main/resources/application.yml`:

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: YOUR_ACTUAL_CLIENT_ID_HERE
            client-secret: YOUR_ACTUAL_CLIENT_SECRET_HERE
```

**Replace:**
- `YOUR_ACTUAL_CLIENT_ID_HERE` with your Client ID (e.g., `123456789-abc.apps.googleusercontent.com`)
- `YOUR_ACTUAL_CLIENT_SECRET_HERE` with your Client Secret

### 7. Restart Your Application

After updating the configuration:
1. Stop your Spring Boot application
2. Restart it with `mvn spring-boot:run`
3. Try logging in again

## Important Notes

⚠️ **Redirect URI Must Match Exactly**
- The redirect URI in Google Console must be: `http://localhost:8080/login/oauth2/code/google`
- Must match exactly (including protocol http://, no trailing slash)

⚠️ **If Using Different Ports**
- If your backend runs on a different port, update the redirect URI accordingly
- Update it in both Google Console and `application.yml`

⚠️ **Client Secret**
- Keep your client secret secure
- Don't commit it to version control
- Consider using environment variables or `application.properties` (which should be in `.gitignore`)

## Troubleshooting

**Error: "OAuth client was not found"**
- Verify Client ID is correct (no extra spaces)
- Make sure you copied the entire Client ID

**Error: "redirect_uri_mismatch"**
- Verify redirect URI in Google Console matches exactly: `http://localhost:8080/login/oauth2/code/google`
- Check for typos, extra spaces, or trailing slashes

**Error: "invalid_client"**
- Check that both Client ID and Client Secret are correct
- Make sure there are no extra spaces when copying

## Alternative: Using application.properties

Instead of `application.yml`, you can create `application.properties`:

```properties
spring.security.oauth2.client.registration.google.client-id=YOUR_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_CLIENT_SECRET
```

This file should be in `.gitignore` to keep credentials secure.

