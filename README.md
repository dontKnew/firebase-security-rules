Here are the **Main Rules** and examples for each:

### == MAIN RULES ==

#### 1. Users Can Access Own Data
Each user can only access their own data. This rule ensures that users can't read or modify other users' data.
```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth.uid == $uid",
        ".write": "auth.uid == $uid"
      }
    }
  }
}
```
- In this rule, `$uid` refers to the user's unique ID. Only the authenticated user with a matching `auth.uid` can read or write their own data.

#### 2. Any Authenticated User Can Access Data
All authenticated users can read and write to the data. This rule is useful for public or shared data where access is given to any logged-in user.
```json
{
  "rules": {
    "publicData": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```
- This allows any authenticated user to read or write the data under the `publicData` node.

#### 3. Admin Access Data
Only users with an admin role can access or modify the data.
```json
{
  "rules": {
    "adminContent": {
      ".read": "auth.token.role == 'admin'",
      ".write": "auth.token.role == 'admin'"
    }
  }
}
```
- This rule checks if the authenticated user has the `admin` role, allowing access only to admins.

---


#### 4. UnAuthentication (without login ) Access Data
Only users with an admin role can access or modify the data.
```json
{
  "rules": {
    "openData": {
      ".read": true,  // Anyone can read this data
      ".write": true  // Anyone can write this data
    }
  }
}
```
- This rule checks if the authenticated user has the `admin` role, allowing access only to admins.

---

### == OPTIONAL RULES ==

#### 1. Validate Data Structure Example
This rule validates the structure of incoming data, ensuring that it follows a specific format or requirement. Here's an example that validates an age field to ensure it's a number and greater than zero.
```json
{
  "rules": {
    "users": {
      "$uid": {
        "profile": {
          ".validate": "newData.hasChildren(['age', 'email']) && newData.child('age').isNumber() && newData.child('age').val() > 0 && newData.child('email').isString() && newData.child('email').val().matches(/^[^@\\s]+@[^@\\s]+\\.[^@\\s]+$/)"
        }
      }
    }
  }
}
```
- This rule ensures that:
  - `age` is a number and greater than zero.
  - `email` is a string and follows a valid email format.
  
This validation ensures data integrity and prevents invalid data from being written into the database.
