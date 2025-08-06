# DIVA Vulnerabilities

This document lists 5 security vulnerabilities identified in the DIVA (Damn Insecure and Vulnerable App) Android project.  
Each entry includes the file, vulnerable code snippet, explanation, and associated risk.

---

## 1. Hardcoded Secrets
**File:** `APICredsActivity`

**Code Snippet:**
```java
String apidetails = "API Key: 123secretapikey123\nAPI User name: diva\nAPI Password: p@ssword";
```

**Issue:**
- Sensitive API credentials are hardcoded directly into the application.
- Attackers can decompile the APK to retrieve these secrets.

**Risk:**
- API keys can be misused for unauthorized access.
- Sensitive information is exposed in plaintext.


## 2. Insecure SharedPreferences Storage
**File:** `InsecureDataStorage1Activity.java`

**Code Snippet:**
```java
SharedPreferences spref = PreferenceManager.getDefaultSharedPreferences(this);
SharedPreferences.Editor spedit = spref.edit();
```

**Issue:**
- Credentials or sensitive data stored in `SharedPreferences` are unencrypted.
- Can be easily extracted from rooted devices.

**Risk:**
- Attackers with device access can steal stored credentials.


## 3. Exported Content Provider
**File:** `AndroidManifest.xml`

**Code Snippet:**
```xml
<provider
android:name=".NotesProvider"
android:authorities="jakhar.aseem.diva.provider.notesprovider"
android:enabled="true"
android:exported="true" />
```

**Issue:**
- Content provider is exported without permissions.
- Other apps can query or modify notes without authorization.
 
**Risk:**
- Data leakage or unauthorized modification of application data.
 

## 4. Insecure Logging
**File:** `LogActivity.java`

**Code Snippet:**
```java
Log.e("diva-log", "Error while processing transaction with credit card: " + cctxt.getText().toString());
```

**Issue:**
- Logs sensitive credit card data in plaintext.
- Device logs can be accessed by other apps or via `adb logcat`.

**Risk:**
- Sensitive data may be stolen if logs are collected.


## 5. Insecure Temporary File Storage
**File:** `InsecureDataStorage3Activity.java`

**Code Snippet:**
```java
File uinfo = File.createTempFile("uinfo", "tmp", ddir);
uinfo.setReadable(true);
uinfo.setWritable(true);
FileWriter fw = new FileWriter(uinfo);
fw.write(usr.getText().toString() + ":" + pwd.getText().toString() + "\n");
fw.close();
```

**Issue:**
- Stores sensitive credentials in world-readable temporary files.
- Data is unencrypted and can be accessed by other apps or on a rooted device.

**Risk:**
- Credentials can be leaked from temporary storage.
