# Firebase Web Codelab



## Important links

- [Code on Github](https://github.com/firebase/friendlychat-web)
- Project on Firebase Console: friendlychat2-3c045
- The completed project is deployed at [this url](https://friendlychat2-3c045.firebaseapp.com/)





## 1. Overview

In this codelab, you'll learn how to use the [Firebase platform](http://firebase.google.com/) to easily create Web applications and you will implement and deploy a chat client using Firebase.

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/eb22bdc11ce003e0.png)

### What you'll learn

- Sync data using the Firebase Realtime Database and Cloud Storage.
- Authenticate your users using Firebase auth.
- Deploy your web app on Firebase static hosting.
- Send notifications with Firebase Cloud Messaging.

### What you'll need

- The IDE/text editor of your choice such as [WebStorm](https://www.jetbrains.com/webstorm), [Atom](https://atom.io/) or [Sublime](https://www.sublimetext.com/).
- [npm](https://www.npmjs.com/) which typically comes with [NodeJS](https://nodejs.org/en/).
- A console.
- A browser such as Chrome.
- The sample code. See next step for





## 2. Get the sample code

Clone the [GitHub repository](https://github.com/firebase/friendlychat-web) from the command line:

```
git clone https://github.com/firebase/friendlychat-web
```

The `friendlychat-web` repository contains sample projects for multiple platforms. This codelab only uses these two repositories:

- 📁 **web-start**—The starting code that you'll build upon in this codelab.
- 📁 **web**—The complete code for the finished sample app.

**Note**: If you would like to run the finished app, you will still have to create a project in the Firebase console, see the **Create a Firebase project and Set up your app** section for instructions.

### Import the starter app

Using your IDE open or import the 📁 `web-start` directory from the sample code directory. This directory contains the starting code for the codelab which consists of fully functional Chat Web App.



## 3. Create a Firebase project and Set up your project

### **Create project**

In the [Firebase console](https://console.firebase.google.com/) click on **Add Project** and call it **FriendlyChat**.

Click **Create Project**.

Note that while your project will be called **FriendlyChat** it will be assigned a unique ID in the form **friendlychat-1234**which is what is ultimately used to identify your project.

The application that we are going to build uses the whole set of Firebase products available on the web:

- **Firebase Authentication** to easily let your users sign-in your app.
- The **Firebase Realtime Database** to save structured data on the Cloud and get instant notification when the data is updated.
- **Cloud Storage for Firebase** to save files in the cloud.
- **Firebase Hosting** to host and serve your static assets.
- **Firebase Cloud Messaging** to send push notifications and display browser popup notifications.

Some of these products need special configuration or need to be enabled using the Firebase console:

### **Enable Google Auth**

To let users sign-in the app we'll use Google auth which needs to be enabled.

In the Firebase Console open the **Develop** section > **Authentication** > **SIGN IN METHOD** tab (or [click here](https://console.firebase.google.com/project/_/authentication/providers) to go there) you need to enable the **Google** Sign-in Provider and click **SAVE**. This will allow users to sign-in the Web app with their Google accounts.

Also feel free to set the public facing name of your app to **Friendly Chat**:

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/8290061806aacb46.png)

### **Enable The Realtime Database**

The app uses the Firebase Realtime database to save the chat messages and receive new chat messages. To enable The Realtime Database on your Firebase project visit the **Database** section and click the **Create database** button in the **Realtime Database** box:

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/8d0191f61077ec1e.png)

Then select the **Start in test mode** option and click **Enable** when you receive the disclaimer about the security rules. This will ensure that we can freely write to the database. We'll make this more secure later on.

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/48cd87a25c56a1aa.png)

### **Enable Cloud Storage**

The app uses Cloud Storage to upload pics. To enable Cloud Storage on your Firebase project visit the **Storage** section and click the **Get Started** button.

Then click **Got It** when you receive the disclaimer about the security rules. With the default security rules, any authenticated users can write to anything to Cloud Storage. We'll make this more secure later on.

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/328ca996218b2e41.png)





## 4. Install the Firebase Command Line Interface

The Firebase Command Line Interface (CLI) will allow you to serve your web apps locally and deploy your web app to Firebase hosting.

To install the CLI you need to have installed [npm](https://www.npmjs.com/) which typically comes with [NodeJS](https://nodejs.org/en/).

To install the CLI run the following npm command:

```
npm -g install firebase-tools
```

Doesn't work? You may need to [change npm permissions.](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

To verify that the CLI has been installed correctly open a console and run:

```
firebase --version
```

Make sure the version of the Firebase CLI is above 3.3.0

Authorize the Firebase CLI by running:

```
firebase login
```

Once authorized, go back to the console and make sure you are in the `web-start` directory. Then set up the Firebase CLI to use your Firebase Project:

```
firebase use --add
```

You will be prompted to select your Project ID. Follow the instructions.

> SM 
>
> ```bash
> ? Which project do you want to add? friendlychat-3d697
> ? What alias do you want to use for this project? (e.g. staging) staging
> 
> Created alias staging for friendlychat-3d697.
> Now using alias staging (friendlychat-3d697)
> ```
>
>



## 5. Run the starter app

Now that you have imported and configured your project you are ready to run the app for the first time. Open a console at the `web-start` folder and run `firebase serve`:

```
firebase serve --only hosting
```

This command should display this in the console:

```
✔  hosting: Local server: http://localhost:5000
```

We're using the [Firebase hosting](https://firebase.google.com/docs/hosting/) emulator to serve the application locally. The web app should now be available from [http://localhost:5000](http://localhost:5000/). Open it. You should see your app's not (yet!) functioning UI:

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/62b7642009abef22.png)

The app cannot do anything right now but with your help it will soon! We only have laid out the UI for you so far. Let's now build a realtime chat!



> SM the layout, CSS and image icon are screwed up at this point.



## 6. Import and Configure Firebase

### **Import the Firebase SDK**

The Firebase SDK needs to be imported in your application. There are multiple ways to do this that are [described in our documentation](https://firebase.google.com/docs/web/setup). You can import the library from our CDN or import it locally using NPM and package it in your application if you're using Browserify. Since we're using Firebase hosting to serve our application we're going to import the local URLs in the `index.html`. We have already added the following for you at the bottom of the `index.html` file but please double check that it is there:

### [index.html](https://github.com/firebase/friendlychat/blob/master/web/index.html#L110-L117.html)

```
<script src="/__/firebase/5.1.0/firebase-app.js"></script>
<script src="/__/firebase/5.1.0/firebase-auth.js"></script>
<script src="/__/firebase/5.1.0/firebase-database.js"></script>
<script src="/__/firebase/5.1.0/firebase-storage.js"></script>
<script src="/__/firebase/5.1.0/firebase-messaging.js"></script>
```

We are going to use Firebase Auth, the Realtime Database, Cloud Storage and Cloud Messaging during this codelab so we're importing all these library. In your future apps, make sure you are only importing the parts of Firebase that you need to lighten the load time of your app.

### **Configure Firebase**

We also need to configure the Firebase SDK to tell it which project we're using. Since we're using Firebase Hosting there is a special script you can import which will do that just for you. Again, we have already added the following for you at the bottom of the `index.html` file but please double check that it is there:

### [index.html](https://github.com/firebase/friendlychat/blob/master/web/index.html#L118.html)

```
<script src="/__/firebase/init.js"></script>
```

This script contains your project configuration based of the project that you have specified earlier using `firebase use --add`

Feel free to inspect that file to see what your project configuration looks like. For this open <http://localhost:5000/__/firebase/init.js> and you should see something that looks like this:

### [/__/firebase/init.js](https://friendlychat-1234.firebaseapp.com/__/firebase/init.js)

```
if (typeof firebase === 'undefined') throw new Error('hosting/init-error: Firebase SDK not detected. You must include it before /__/firebase/init.js');
firebase.initializeApp({
  "apiKey": "qwertyuiop_asdfghjklzxcvbnm1234568_90",
  "databaseURL": "https://friendlychat-1234.firebaseio.com",
  "storageBucket": "friendlychat-1234.appspot.com",
  "authDomain": "friendlychat-1234.firebaseapp.com",
  "messagingSenderId": "1234567890",
  "projectId": "friendlychat-1234"
});
```

Instead of using the locally available implicit configuration that's generated by Firebase hosting you could have also copied the initialization snippet which you can find in the Firebase console of your project, in the Project Overview > Add Firebase to your Web App:

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/36f835c9da595e23.png)

You should use that especially if you are using your own hosting server rather than Firebase Hosting.



## 7. User Sign-in

The Firebase SDK should now be ready to use since it is imported and initialized it in the `index.html` file. We're first going to implement User sign-in using [Firebase Auth](https://firebase.google.com/docs/auth/web/start).

### **Authenticate your users with Google Sign-In**

When the user clicks the **Sign in with Google** button the `signIn` function gets triggered (we already set that up for you!). At this point we want to authorize Firebase using Google as the Identity Provider. We'll sign in using a popup ([Several other methods](https://firebase.google.com/docs/auth/web/google-signin) are available). Change the `signIn` function with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L18-L23.js)

```
// Signs-in Friendly Chat.
function signIn() {
  // Sign in Firebase using popup auth and Google as the identity provider.
  var provider = new firebase.auth.GoogleAuthProvider();
  firebase.auth().signInWithPopup(provider);
}
```

The `signOut` function is triggered when the user clicks the **Sign out** button. Add the following line to make sure we sign out of Firebase:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L25-L29.js)

```
// Signs-out of Friendly Chat.
function signOut() {
  // Sign out of Firebase.
  firebase.auth().signOut();
}
```

### **Track the auth state**

We need a way to check if the user is signed-in or signed-out in order to update the UI accordingly. With Firebase auth you can register an observer on the auth state which will be triggered every time there is a change in the auth state. Change the `initFirebaseAuth` function in the `scripts/main.js` to this:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L31-L35.js)

```
// Initiate firebase auth.
function initFirebaseAuth() {
  // Listen to auth state changes.
  firebase.auth().onAuthStateChanged(authStateObserver);
}
```

This registers the function `authStateObserver` as the auth state observer. It will trigger every time there is a change in the auth state, when the user signs-in or signs-out. That's where we'll update the UI to display or hide the Sign-in button, the Sign-out button the signed-in user's profile pic... All the UI part has already been implemented

### **Display the signed-in user information**

We want to display the signed-in user's profile pic and name in the top bar. In Firebase, the signed in user's data is always available in the `firebase.auth().currentUser` object. Earlier we've set up the `authStateObserver` function to trigger when the user signs-in so that it updates the UI accordingly. It will call the `getProfilePicUrl` and `getUserName` when triggered. Change these two functions with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L37-L45.js)

```
// Returns the signed-in user's profile Pic URL.
function getProfilePicUrl() {
  return firebase.auth().currentUser.photoURL || '/images/profile_placeholder.png';
}

// Returns the signed-in user's display name.
function getUserName() {
  return firebase.auth().currentUser.displayName;
}
```

We display an error message if the users tries to send messages when the user is not signed-in (you can try!). To detect if the user is actually signed-in, change the `isUserSignedIn` function to:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L47-L50.js)

```
// Returns true if a user is signed-in.
function isUserSignedIn() {
  return !!firebase.auth().currentUser;
}
```

### Test Signing-in to the app

1. Reload your app if it is still being served or run `firebase serve` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) and open it in your browser.
2. Sign-In using the Sign In button
3. After Signing in the profile pic and name of the user should be displayed:
4. ![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/8856f18e295de3f2.png)



> SM - again the profile pic isn't showing and the layout is broken
> 





## 8. Read messages

### Import Messages

To store the chat messages that are written by users we'll use the [Firebase Realtime Database](https://firebase.google.com/docs/database/).

To show how to read from the database we'll start by adding a few test messages in the Firebase Realtime Database.

In your project's Firebase console visit the **Database** section on the left navigation bar. On this page you will see the data that is currently stored in your Firebase Realtime Database.

In the overflow menu of the Database select **Import JSON**. Browse to the [`initial_messages.json`](https://github.com/firebase/friendlychat/blob/master/initial_messages.json) file at the root of the repository, select it then click the **Import** button. This will replace any data currently in your database.

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/b8c1fd1db2141cbd.png)

You could also edit the database manually, using the green + and red x to add and remove items manually or use the Firebase CLI with this command:

```
firebase database:set / ../initial_messages.json
```

After importing the JSON file your database should contain the following elements:

```
friendlychat-12345/
    messages/
        -K2ib4H77rj0LYewF7dP/
            text: "Hello"
            name: "anonymous"
        -K2ib5JHRbbL0NrztUfO/
            text: "How are you"
            name: "anonymous"
        -K2ib62mjHh34CAUbide/
            text: "I am fine"
            name: "anonymous"
```

These are a few sample chat messages to get us started with reading from the Database.

### Synchronize **Messages**

To synchronize messages on the app we'll need to add listeners that trigger when changes are made to the data and then create a UI element that show new messages.

Add code that listens to newly added messages to the app's UI. To do this, modify the `loadMessages` function. This is where we'll register the listeners that listen to changes made to the data. We'll only display the last 12 messages of the chat to avoid displaying a very long history on load.

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L52-L62.js)

```
// Loads chat messages history and listens for upcoming ones.
function loadMessages() {
  // Loads the last 12 messages and listen for new ones.
  var callback = function(snap) {
    var data = snap.val();
    displayMessage(snap.key, data.name, data.text, data.profilePicUrl, data.imageUrl);
  };

  firebase.database().ref('/messages/').limitToLast(12).on('child_added', callback);
  firebase.database().ref('/messages/').limitToLast(12).on('child_changed', callback);
}
```

When listening to messages in the database we first have to specify under which path is the data we want to listen to using the .`ref` function. Above we're listening to the changes under the **/messages/** path which is where the messages are stored. We're also applying a limit and only listening to the last 12 messages using `.limitToLast(12)`

The callback function - `callback` - is specified using the `.on` function and by also specifying the event type we want to listen to. In this case our callback function will be called when a new message has been added - using the `child_added` event type - or an existing message has been modified - using the `child_changed` event type. There are a few other events you can listen to such as when a child has been deleted or moved. Feel free to read more about this [in our documentation](https://firebase.google.com/docs/database/web/lists-of-data#listen_for_child_events).

### Test Message Sync

1. Reload your app if it is still being served or run `firebase serve` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) and open it in your browser.
2. The sample messages we imported earlier into the database should be displayed in the Friendly-Chat UI (see below). You can also manually modify or add new messages directly from the **Database** section of the Firebase console. Congratulations, you are reading real-time database entries in your app!
   ![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/af5bbc17f5c16d85.png)





## 9. Send Messages

### **Implement Message Sending**

In this section you will add the ability for users to send messages. The code snippet below is triggered upon clicks on the **SEND** button and pushes a message object with the contents of the message field to the Firebase database under the **/messages/** path. The `push()` method adds an automatically generated key to the pushed object's path. These keys are sequential which ensures that the new messages will be added to the end of the list. Update the `saveMessage` function with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L64-L74.js)

```
// Saves a new message on the Firebase DB.
function saveMessage(messageText) {
  // Add a new message entry to the Firebase Database.
  return firebase.database().ref('/messages/').push({
    name: getUserName(),
    text: messageText,
    profilePicUrl: getProfilePicUrl()
  }).catch(function(error) {
    console.error('Error writing new message to Firebase Database', error);
  });
}
```

### **Test Sending Messages**

1. Reload your app if it is still being served or run `firebase serve` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) and open it in your browser.
2. After signing-in, enter a message and hit the send button, the new message should be visible in the app UI and in the Firebase console with your user photo and name:
   ![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/5525d51569916a34.png)



> SM - ok can send a message



## 10. Send Images

We'll now add a feature that shares images. While the Realtime Database is good to store structured data, files are better stored in Cloud Storage. [Cloud Storage for Firebase](https://firebase.google.com/docs/storage/) is a file/blob storage service and we'll use it to store the images the user shares.

### Save images to Cloud Storage

We have already added for you a button that triggers a file picker dialog. After selecting a file the `saveImageMessage`function is called and you can get a reference to the selected file. Now we'll add code that:

1. Creates a "placeholder" chat message into the chat feed, so that users see a "Loading" animation while we upload the image.
2. Upload the image file to Cloud Storage to the path: `/<uid>/<messageId>/<file_name>`
3. Generate a publicly readable URL for the image file.
4. Update the chat message with the newly uploaded image file's URL in lieu of the temporary loading image.

Modify the `saveImageMessage` function with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L76-L100.js)

```
// Saves a new message containing an image in Firebase.
// This first saves the image in Firebase storage.
function saveImageMessage(file) {
  // 1 - We add a message with a loading icon that will get updated with the shared image.
  firebase.database().ref('/messages/').push({
    name: getUserName(),
    imageUrl: LOADING_IMAGE_URL,
    profilePicUrl: getProfilePicUrl()
  }).then(function(messageRef) {
    // 2 - Upload the image to Cloud Storage.
    var filePath = firebase.auth().currentUser.uid + '/' + messageRef.key + '/' + file.name;
    return firebase.storage().ref(filePath).put(file).then(function(fileSnapshot) {
      // 3 - Generate a public URL for the file.
      return fileSnapshot.ref.getDownloadURL().then((url) => {
        // 4 - Update the chat message placeholder with the image's URL.
        return messageRef.update({
          imageUrl: url,
          storageUri: fileSnapshot.metadata.fullPath
        });
      });
    });
  }).catch(function(error) {
    console.error('There was an error uploading a file to Cloud Storage:', error);
  });
}
```

### **Test Sending images**

1. Reload your app if it is still being served or run `firebase serve` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) then open it in your browser.

2. After signing-in, click the image upload button:![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/a5941d3daaa70956.png) and select an image file using the file picker, a new message should be visible in the app UI with your selected image:

3. > SM -  That image icon isn't showing up....

4. ![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/eb22bdc11ce003e0.png)

5. If you try adding an image while not signed-in you should see a Toast telling you that you must sign in in order to add images.





> SM - can't upload when not logged in...  ok



## 11. Show Notifications

We'll now add support for browser notifications. This way your users can receive a notification when a new message has been posted in the chat. [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) is a cross-platform messaging solution that lets you reliably deliver messages and notifications at no cost.

### **Whitelist the GCM Sender ID**

In the [web app manifest](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/) you need to specify the `gcm_sender_id`, a hard-coded value indicating that FCM is authorized to send messages to this app. Friendlychat already has a `manifest.json` configuration file so add the browser sender ID in the `gcm_sender_id` attribute exactly as shown below (do not change the value):

### [manifest.json](https://github.com/firebase/friendlychat/blob/master/web/manifest.json#L7.json)

```
{
  "name": "Friendly Chat",
  "short_name": "Friendly Chat",
  "start_url": "/index.html",
  "display": "standalone",
  "orientation": "portrait",
  "gcm_sender_id": "103953800507"
}
```

### **Add the Firebase Messaging service worker**

The web app needs a [Service Worker](https://developer.mozilla.org/en/docs/Web/API/Service_Worker_API) that will receive and display web notifications. Create a file named `firebase-messaging-sw.js` at the root of your project with the following content:

### [firebase-messaging-sw.js](https://github.com/firebase/friendlychat/blob/master/web/firebase-messaging-sw.js#L4-L9.json)

```
importScripts('/__/firebase/5.1.0/firebase-app.js');
importScripts('/__/firebase/5.1.0/firebase-messaging.js');
importScripts('/__/firebase/init.js');

firebase.messaging();
```

The service worker simply need to load and initialize the Firebase Cloud Messaging SDK which will take care of displaying notifications.

### **Get FCM device tokens**

When notifications have been enabled on a device or browser, you'll be given a **device token**. This device token is what we use to send a notification to a particular device or particular browser.

When the user signs-in we call the `saveMessagingDeviceToken` function. That's where we'll get the **FCM device token** from the browser and save it to the Realtime Database.

Update the `saveMessagingDeviceToken` function with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L102-L117.js)

```
// Saves the messaging device token to the datastore.
function saveMessagingDeviceToken() {
  firebase.messaging().getToken().then(function(currentToken) {
    if (currentToken) {
      console.log('Got FCM device token:', currentToken);
      // Saving the Device Token to the datastore.
      firebase.database().ref('/fcmTokens').child(currentToken)
          .set(firebase.auth().currentUser.uid);
    } else {
      // Need to request permissions to show notifications.
      requestNotificationsPermissions();
    }
  }).catch(function(error){
    console.error('Unable to get messaging token.', error);
  });
}
```

However this will not work initially. For your app to be able to retrieve the device token the user needs to grant your app permission to show notifications.

### **Request permissions to show notifications**

When the user has not yet granted you permission to show notifications you will not be given a device token. In this case we call the `firebase.messaging().requestPermission()` method which will display a browser dialog asking for this permission [in supported browsers](https://caniuse.com/#feat=push-api):

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/93ec8bf8a1fea7e1.png)

Update the `requestNotificationsPermissions` function with:

### [main.js](https://github.com/firebase/friendlychat/blob/master/web/scripts/main.js#L119-L128.js)

```
// Requests permissions to show notifications.
function requestNotificationsPermissions() {
  console.log('Requesting notifications permission...');
  firebase.messaging().requestPermission().then(function() {
    // Notification permission granted.
    saveMessagingDeviceToken();
  }).catch(function(error) {
    console.error('Unable to get permission to notify.', error);
  });
}
```

### **Get your Device token**

1. Reload your app if it is still being served or run `firebase serve` on the command line to start serving the app from [http://localhost:5000](http://localhost:5000/) then open it in your browser.
2. After signing-in, you should see the Notifications permission dialog being displayed:
   ![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/4169287ac98921d0.png)
3. Click **Allow** and open the JavaScript console of your browser, you should see a message that reads:
   `Got FCM device token: cWL6w:APA91bHP...4jDPL_A-wPP06GJp1OuekTaTZI5K2Tu`
4. Copy your device token, you will need it for the next step.



### **Send a notification to your device**

Now that you have your device token you can send a notification. You will also need your Firebase app's **Server Key**. to get it open your app's [**Firebase Console > Project Settings > Cloud Messaging**](https://console.firebase.google.com/project/_/settings/cloudmessaging) and copy the **Server Key**.



To send a notification you need to send the following HTTP request:

```
POST /fcm/send HTTP/1.1
Host: fcm.googleapis.com
Content-Type: application/json
Authorization: key=YOUR_SERVER_KEY

{
  "notification": {
    "title": "New chat message!",
    "body": "There is a new message in FriendlyChat",
    "icon": "/images/profile_placeholder.png",
    "click_action": "http://localhost:5000"
  },
  "to":"YOUR_DEVICE_TOKEN"
}
```

You can do this by using [cURL](https://curl.haxx.se/) using the following command line:



```
curl -H "Content-Type: application/json" \
     -H "Authorization: key=YOUR_SERVER_KEY" \
     -d '{
           "notification": {
             "title": "New chat message!",
             "body": "There is a new message in FriendlyChat",
             "icon": "/images/profile_placeholder.png",
             "click_action": "http://localhost:5000"
           },
           "to": "YOUR_DEVICE_TOKEN"
         }' \
     https://fcm.googleapis.com/fcm/send
```



Don't forget to replace `YOUR_SERVER_KEY` and `YOUR_DEVICE_TOKEN` with your **Server Key** and your **Device Token**.

If you do not have cURL or if it's hard to install on your machine - for instance on Windows - feel free to use this service: [onlinecurl.com](https://onlinecurl.com/)

Beware that sharing your Server Key with a third party service may not be secure. Make sure to refresh your Server Key after test/development.

The notification will only appear if your FriendlyChat app is in the background. You must, for instance, navigate away or display another tab for the notification to be displayed.

When the app is in the foreground, you can [catch the messages sent by FCM](https://firebase.google.com/docs/cloud-messaging/js/receive#handle_messages_when_your_web_app_is_in_the_foreground).

If your app is on the background you should see a notification appear such as:

![img](https://codelabs.developers.google.com/codelabs/firebase-web/img/a2fe01ddc075e45a.png)

In the follow-up codelab [**Firebase SDK for Cloud Functions**](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions) we'll see how you can automate sending notifications from a backend for each new messages posted in the chat.



> SM - ok seems to be working



## 12. Database Security Rules [Optional]

### **Set Database Security Rules**

The Firebase Realtime Database uses a specific [rules language](https://firebase.google.com/docs/database/security/) to define access rights, security and data validations.

When setting up your Firebase project at the beginning of this codelab, we've chosen default security rules that do not restrict access to the database. You can view and modify these rules in the **Database** section of [Firebase console](https://console.firebase.google.com/) under the **RULES** tab. You should be seeing the default rules:

```
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

These rule do not restrict access to the database. This means any user can read and write to any path in your database.

Right in the Update the rules to the following which will restrict things a bit:

### [database-rules.json](https://github.com/firebase/friendlychat/blob/master/web/database-rules.json)

```
{
  "rules": {
    "messages": {
      ".read": true,
      // Users can only write messages if they are authenticated.
      ".write": "auth !== null",
      "$messageId": {
        // The name has to be the real display name.
        ".validate": "newData.child('name').val() === auth.token.name",
        "text": {
          // The text has to be less than 300 char.
          ".validate": "newData.isString() && newData.val().length < 300"
        }
      }
    },
    "fcmTokens": {
      // User can write their tokens to the database but not read the list of tokens.
      ".read": false,
      ".write": true
    }
  }
}
```

The `auth` rule variable is a special variable containing information about the user if authenticated. More information can be found in [the documentation](https://firebase.google.com/docs/database/security/).

You can update these rules manually directly in the Firebase console. Alternatively you can write these rules in a file and apply them to your project using the CLI:

- Save the rules shown above in a file named `database-rules.json`.
- In your `firebase.json` file, add a `database.rules` attribute pointing to a JSON file containing the rules shown above (the `hosting` attribute should already be there):

### [firebase.json](https://github.com/firebase/friendlychat/blob/master/web/firebase.json#L2-L4.json)

```
{
  "database": {
    "rules": "database-rules.json"
  },
  "hosting": {
    "public": "./",
    "ignore": [
      "firebase.json",
      "database-rules.json",
      "storage.rules",
      "functions"
    ]
  }
}
```

### **Deploy Database Security Rules**

You can then deploy these rules with the Firebase CLI using `firebase deploy --only database`:

```
firebase deploy --only database
```

This is the console output you should see:

```
=== Deploying to 'friendlychat-1234'...

i  deploying database
i  database: checking rules syntax...
✔  database: rules syntax for database friendlychat-1234 is valid
i  database: releasing rules...
✔  database: rules for database friendlychat-1234 released successfully

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```





> SM - ok





## 13. Storage Security Rules [Optional]

### **Set Storage Security Rules**

Cloud Storage for Firebase uses a specific [rules language](https://firebase.google.com/docs/storage/security/start) to define access rights, security and data validations.

New Firebase projects are set up with default rules that only allow authenticated users to use Cloud Storage. You can view and modify these rules in the **Storage** section of [Firebase console](https://firebase.google.com/console) under the **RULES** tab. You should be seeing the following default rule:

```
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

This rule allow any signed-in user to read and write any files in your Storage bucket.

Update the rules to the following which only allows users to write to their own folder:

### [storage.rules](https://github.com/firebase/friendlychat/blob/master/web/storage.rules)

```
service firebase.storage {
  match /b/{bucket}/o {
    match /{userId}/{messageId}/{fileName} {
      allow write: if request.auth.uid == userId;
      allow read;
    }
  }
}
```

The `request.auth` rule variable is a special variable containing information about the user if authenticated. More information can be found in [the documentation](https://firebase.google.com/docs/storage/security/start).

You can update these rules manually directly in the Firebase console. Alternatively you can write these rules in a file and apply them to your project using the CLI:

- Save the rules shown above in a file named `storage.rules`
- In your `firebase.json` file, add a `storage.rules` attribute pointing to a file containing the rules shown above:

### [firebase.json](https://github.com/firebase/friendlychat/blob/master/web/firebase.json#L5-L7.json)

```
{
  // If you went through the "Realtime Database Security Rules" step.
  "database": {
    "rules": "database-rules.json"
  },
  "storage": {
    "rules": "storage.rules"
  },
  "hosting": {
    "public": "./",
    "ignore": [
      "firebase.json",
      "database-rules.json",
      "storage.rules",
      "functions"
    ]
  }
}
```

### **Deploy Storage Security Rules**

You can then deploy these rules with the Firebase CLI using `firebase deploy --only storage`:

```
firebase deploy --only storage
```

This is the console output you should see:

```
=== Deploying to 'friendlychat-1234'...

i  deploying storage
i  storage: checking storage.rules for compilation errors...
✔  storage: rules file storage.rules compiled successfully
i  storage: uploading rules storage.rules...
✔  storage: released rules storage.rules to firebase.storage/friendlychat-1234.appspot.com

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```





## 14. Deploy your app using Firebase static hosting

Firebase comes with a [hosting service](https://firebase.google.com/docs/hosting/) that will serve your static assets/web app. You deploy your files to Firebase Hosting using the Firebase CLI. Before deploying you need to specify which files will be deployed in your `firebase.json` file. We have already done this for you because this was required to serve the file for development through this codelab. These settings are specified under the `hosting` attribute:

### [firebase.json](https://github.com/firebase/friendlychat/blob/master/web/firebase.json#L8-L15.json)

```
{
  // If you went through the "Realtime Database Security Rules" step.
  "database": {
    "rules": "database-rules.json"
  },
  // If you went through the "Storage Security Rules" step.
  "storage": {
    "rules": "storage.rules"
  },
  "hosting": {
    "public": "./",
    "ignore": [
      "firebase.json",
      "database-rules.json",
      "storage.rules",
      "functions"
    ]
  }
}
```

This will tell the CLI that we want to deploy all files in the current directory ( `"public": "./"` ) with the exception of the files listed in the `ignore` array.

Now deploy your files to Firebase static hosting by running `firebase deploy`:

```
firebase deploy --except functions
```

This is the console output you should see:

```
=== Deploying to 'friendlychat-1234'...

i  deploying database, storage, hosting
i  database: checking rules syntax...
✔  database: rules syntax for database friendlychat-1234 is valid
i  storage: checking storage.rules for compilation errors...
✔  storage: rules file storage.rules compiled successfully
i  storage: uploading rules storage.rules...
i  hosting: preparing ./ directory for upload...
✔  hosting: 14 files uploaded successfully
i  database: releasing rules...
✔  database: rules for database friendlychat-1234 released successfully
✔  storage: released rules storage.rules to firebase.storage/friendlychat-1234.appspot.com

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
Hosting URL: https://friendlychat-12345.firebaseapp.com
```

Then simply visit your web app hosted on Firebase Hosting on `https://<project-id>.firebaseapp.com` or by running `firebase open hosting:site`.

The **Hosting** tab in the Firebase console will display useful information such as the history of your deploys with the ability to rollback to previous versions and the ability to set up a custom domain.