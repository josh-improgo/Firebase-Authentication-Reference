# Firebase Authentication
## 1. Install firebase into your project

```npm
npm install firebase
```
## 2. Set up Project on Google Firebase
1. Go to the Console of Google Firebase
2. Go to the project settings of your project
![](https://i.imgur.com/ImRj2HL.png)
3. Under "Your Apps", add a Web App
<!-- 4. Copy the Firebase SDK snippet config -->
You will then see the Firebase SDK snippet. This snippet will be used later.
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```
## 3. Create Folders and Files in your Project
1. In your project's **src directory**, create a ***new folder*** called "**config**".
    a. Inside the **config** directory, create a ***new file*** called "**firebase-config.js**".
    b. Inside the **config** directory, create a ***new file*** called "**authentication-methods.js**".
2. In your project's **src directory**, create a ***new folder*** called "**service**".
    a. Inside the **service** directory, create a ***new file*** called "**auth.js**".


## 4. Populate Files
In the "**firebase-config.js**" file, put in the following code:
```javascript
// firebase-config.js
import firebase from 'firebase'; // allows you to use firebase from node modules

// Copied from the Firebase SDK snippet config
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);
firebase.analytics();

export default firebase;
```
In the "**authentication-methods.js**" file, put in the following code:
```javascript
// authentication-methods.js
import firebase from './firebase-config';

// use Google Authentication
export const googleProvider = new firebase.auth.GoogleAuthProvider();
```


In the "**authentication-methods.js**" file
```javascript
// authentication-methods.js
import firebase from '../config/firebase-config';

// method to pass various providers for different authentication methods
export const socialMediaAuth = provider => (
    firebase
        .auth()
        .signInWithPopup(provider)
        .then(response => response.user)
        .catch(error => error)
);
```

Note: When using firebase, import "**firebase**" from the path to the "**firebase-config.js**" file so you are using the firebase configurations to ***your app***.

## 5. Sign Up
When **signing up**, create an onClick listener on the button which will handle the Third Party Social Media Authentication.

To handle default sign up with email and password, use the method:
`.createUserWithEmailAndPassword(email, password)`

```jsx
// signup.js
import firebase from '../config/firebase-config';
import { socialMediaAuth } from '../../service/auth'
import { googleProvider } from '../../config/authentication-methods'
// ...
    const handleOnClick = async provider => {
        const response = await socialMediaAuth(provider);
    }
    
    // handles default sign up with email and password authentication
    const handleSubmit = event => {
        event.preventDefault();
        
        firebase
            .auth()
            .createUserWithEmailAndPassword(email, password)
            .then(response => console.log(response))
            .catch(error => console.log(error.message));
    }
    // ...
    return (
        // ...
        // using a button for Google Sign Up
        <button onClick={() => handleOnClick(googleProvider)}>Google</button>
        // ...
    )
```

## 6. Sign In
When **signing in** with Google, reuse the code for **Signing Up**. 

To handle default sign in with email and password, use the method:
`.signInWithEmailAndPassword(email, password)`

```jsx
// signin.js
import firebase from '../config/firebase-config';
import { socialMediaAuth } from '../../service/auth'
import { googleProvider } from '../../config/authentication-methods'
// ...
    const handleOnClick = async provider => {
        const response = await socialMediaAuth(provider);
    }
    
    // handles default sign in with email and password authentication
    const handleSubmit = (event) => {
        event.preventDefault();
        firebase
            .auth()
            .signInWithEmailAndPassword(email, password)
            .then(response => console.log(response))
            .catch(error => console.log(error.message));

    // ...
    return (
        // ...
        // using a button for Google Sign Up
        <button onClick={() => handleOnClick(googleProvider)}>Google</button>
        // ...
    )
}
```
## 7. Sign Out
To handle signing out, use the method:
`.signOut()`
```jsx
import firebase from '../config/firebase-config';
    // ...
    const handleSignOut = () => {
        firebase
            .auth()
            .signOut()
            .then(() => console.log("Sign out successful"))
            .catch(error => console.log(error.message));
    }
    // ...
    return (
        // ...
        // using a button for sign out
        <button onClick={handleSignOut}>Sign Out</button>
        // ...
    )
```
