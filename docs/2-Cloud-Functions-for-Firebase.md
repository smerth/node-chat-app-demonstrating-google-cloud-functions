# Cloud Functions for Firebase



## 1. Overview

In this codelab, you'll learn how to use the Firebase SDK for Google Cloud Functions to improve a Chat Web app and how to use Cloud Functions to send notifications to users of the Chat app.

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/eb22bdc11ce003e0.png)

This codelab is meant as a sequel of the [**Firebase Web Codelab**](https://codelabs.developers.google.com/codelabs/firebase-web) in which you learn how to build a chat app using the Firebase Web SDK. If you would like to learn how to use the Firebase platform on the Web by building a chat app, follow the [**Firebase Web Codelab**](https://codelabs.developers.google.com/codelabs/firebase-web) first. If you already know how to use the Firebase platform and CLI and want to learn about the Firebase SDK for Cloud Functions you're at the right place. We'll show you how to clone the completed **Firebase Web Codelab** below.

### What you'll learn

- Create Google Cloud Functions using the Firebase SDK.
- Trigger Cloud Functions based on Auth, Cloud Storage and Realtime Database events.
- Add Firebase Cloud Messaging support to your web app.

### What you'll need

- The IDE/text editor of your choice such as [WebStorm](https://www.jetbrains.com/webstorm), [Atom](https://atom.io/) or [Sublime](https://www.sublimetext.com/).
- A terminal to run shell commands with NodeJS v8 installed.
- A browser such as Chrome.
- The sample code. See next step for this.

### What you'll learn

- Create a Google Cloud Functions using the Firebase SDK.
- Trigger Cloud Functions based on Auth, Cloud Storage and Realtime Database events.
- Add Firebase Cloud Messaging support to your web app.



## 2. Get the sample code

Clone the [GitHub repository](https://github.com/firebase/friendlychat) from the command line:

```
git clone https://github.com/firebase/friendlychat
```

The firebase-codelabs repository contains sample projects for multiple platforms. This codelab only uses these two repositories:

- ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/bb745dc85ae69f6b.png)**cloud-functions-start**—The starting code that you'll build upon in this codelab. Note: This is the same code as the final code for the Firebase Web Codelab.
- ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/bb745dc85ae69f6b.png)**cloud-functions**—The complete code for the finished sample app.

**Note**: If you would like to run the finished app, you will still have to create a project in the Firebase console. See the **Create a Firebase project and Setup your app** section for instructions.

### Import the starter app

Using your IDE, open or import the ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/bb745dc85ae69f6b.png)`cloud-functions-start` directory from the sample code directory. This directory contains the starting code for the codelab which consists of a fully functional Chat Web App.



## 3. Create a Firebase project and Set up your app

### **Create project**

In the [Firebase console](https://console.firebase.google.com/) click on **Add Project** and call it **FriendlyChat**.

Click **Create Project**.

Note that while your project will be called **FriendlyChat** it will be assigned a unique ID in the form **friendlychat-1234**which is what is ultimately used to identify your project.

### **Enable Google Auth**

To let users sign-in the app we'll use Google auth which needs to be enabled.

In the Firebase Console open the **Develop** section > **Authentication** > **SIGN IN METHOD** tab (or [click here](https://console.firebase.google.com/project/_/authentication/providers) to go there) you need to enable the **Google** Sign-in Provider and click **SAVE**. This will allow users to sign-in the Web app with their Google accounts.

Also feel free to set the public facing name of your app to **Friendly Chat**:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/8290061806aacb46.png)

### **Enable Cloud Storage**

The app uses Cloud Storage to upload pics. To enable Cloud Storage on your Firebase project visit the **Storage** section and click the **Get Started** button. Then Click **Got It** when you receive the disclaimer about the security rules.

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/842ad84821323ef5.png)





## 4. Install the Firebase Command Line Interface

The Firebase Command Line Interface (CLI) will allow you to serve the web app locally and deploy your web app and Cloud Functions.

To install the CLI you need to have installed [npm](https://www.npmjs.com/) which typically comes with [NodeJS](https://nodejs.org/en/).

To install or upgrade the CLI run the following npm command:

```
npm -g install firebase-tools
```

Doesn't work? You may need to [change npm permissions.](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

To verify that the CLI has been installed correctly, open a console and run:

```
firebase --version
```

Make sure the version of the Firebase CLI is above **4.0.0** so that it has all the latest features required for Cloud Functions. If not, run `npm install -g firebase-tools` to upgrade as shown above.

Authorize the Firebase CLI by running:

```
firebase login
```

Make sure you are in the `cloud-functions-start` directory then set up the Firebase CLI to use your Firebase Project:

```
firebase use --add
```

Then select your Project ID and follow the instructions. When prompted, you can choose any Alias, such as `codelab` for instance.





## 5. Deploy and run the web app

Now that you have imported and configured your project you are ready to run the web app for the first time. Open a console at the `cloud-functions-start` folder and run `firebase deploy --except functions` this will only deploy the web app to Firebase hosting:

```
firebase deploy --except functions
```

This is the console output you should see:

```
i deploying database, storage, hosting
✔  database: rules ready to deploy.
i  storage: checking rules for compilation errors...
✔  storage: rules file compiled successfully
i  hosting: preparing ./ directory for upload...
✔  hosting: ./ folder uploaded successfully
✔ storage: rules file compiled successfully
✔ hosting: 8 files uploaded successfully
i starting release process (may take several minutes)...

✔ Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
Hosting URL: https://friendlychat-1234.firebaseapp.com
```

### **Open the web app**

The last line should display the **Hosting URL.** The web app should now be served from this URL which should be of the form https://<project-id>.firebaseapp.com. Open it. You should see a chat app's functioning UI.

Sign-in to the app by using the **SIGN-IN WITH GOOGLE** button and feel free to add some messages and post images:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/eb22bdc11ce003e0.png)

If you sign-in the app for the first time on a new browser make sure you allow notifications when prompted:
![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/93ec8bf8a1fea7e1.png)

We'll need you to have notifications enabled at a later point.

If you have accidentally clicked **Block** you can change this setting by clicking on the **🔒 Secure** button on the left of the URL in the Chrome Omnibar and selecting **Notifications > Always Allow on this Site**:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/4b309f3754532766.png)

You don't see this? If you are in an incognito browser session note that Firebase Cloud Messaging is disabled. To complete this step use a regular browser session.

Now we'll be adding some functionality using the Firebase SDK for Cloud Functions.



## 6. The Functions Directory

Cloud Functions allows you to easily have code that runs in the Cloud without having to setup a server. We'll be showing you how to build functions that react to Firebase Auth, Cloud Storage and Firebase Realtime Database events. Let's start with Auth.

When using the Firebase SDK for Cloud Functions, your Functions code will live under the `functions` directory (by default). Your Functions code is also a [Node.js](https://nodejs.org/) app and therefore needs a `package.json` that gives some information about your app and lists dependencies.

To make it easier for you we've already created the `functions/index.js` file where your code will go. Feel free to inspect this file before moving forward.

```
cd functions
ls
```

If you are not familiar with [Node.js](https://nodejs.org/) it will help to learn more about it before continuing the codelab.

The package.json file already lists two required dependencies: the [Firebase SDK for Cloud Functions](https://www.npmjs.com/package/firebase-functions) and the [Firebase Admin SDK](http://npmjs.com/package/firebase-admin). To install them locally run `npm install` from the `functions` folder:

```
npm install
```

By default Cloud Functions runs your code in a Node 6 runtime. We've changed this to the Node 8 runtime which is in Beta by specifying it in the `package.json` file by adding: `"engines": { "node": "8" }`

Let's now have a look at the `index.js` file:

### index.js

```
/**
 * Copyright 2017 Google Inc. All Rights Reserved.
 * ...
 */

// TODO(DEVELOPER): Import the Cloud Functions for Firebase and the Firebase Admin modules here.

// TODO(DEVELOPER): Write the addWelcomeMessage Function here.

// TODO(DEVELOPER): Write the blurImages Function here.

// TODO(DEVELOPER): Write the sendNotification Function here.
```

We'll first import the required modules and then write three Functions in place of the TODOs. First let's import the required Node modules.





## 7. Import the Cloud Functions and Firebase Admin modules

Two modules will be required during this codelab, the `firebase-functions` module allows us to write the Cloud Functions trigger rules, while the `firebase-admin` module allows us to use the Firebase platform on a server with admin access, for instance to write to the Realtime Database or send FCM notifications.

In the `index.js` file, replace the first `TODO` with the following:

### index.js

```
/**
 * Copyright 2017 Google Inc. All Rights Reserved.
 * ...
 */

// Import the Firebase SDK for Google Cloud Functions.
const functions = require('firebase-functions');
// Import and initialize the Firebase Admin SDK.
const admin = require('firebase-admin');
admin.initializeApp();

// TODO(DEVELOPER): Write the addWelcomeMessage Function here.

// TODO(DEVELOPER): Write the blurImages Function here.

// TODO(DEVELOPER): Write the sendNotification Function here.
```

The Firebase Admin SDK can be configured automatically when deployed on a Cloud Functions environment or other Google Cloud Platform containers. This is what happens do above when using `admin.initializeApp();`

Now let's add a Function that runs when a user signs in for the first time in your chat app and we'll add a chat message to welcome the user.





## 8. Welcome New Users

## **Chat messages structure**

Message posted to the FriendlyChat chat feed are stored in the Firebase Realtime Database. Let's have a look at the data structure we use for a message. To do this, post a new message to the chat that reads "Hello World":

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/f0578fea7cd44b48.png)

This should appear as:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/97c41bfff6343749.png)

In your Firebase app console open the Realtime Database section:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/a536ae925c1ad13e.png)

As you can see, chat messages are stored in the Realtime Database as an object with the `name`, `photoUrl` and `text`attributes added to the `/messages` collection.

## **Adding welcome messages**

The first Cloud Function adds a message that welcomes **new users** into the chat. For this we can use the trigger `functions.auth().onCreate` which runs the function every time a user signs-in for the first time in your Firebase app. Add the `addWelcomeMessages` function into your `index.js` file:

### index.js

```
// Adds a message that welcomes new users into the chat.
exports.addWelcomeMessages = functions.auth.user().onCreate(async (user) => {
  console.log('A new user signed in for the first time.');
  const fullName = user.displayName || 'Anonymous';

  // Saves the new welcome message into the database
  // which then displays it in the FriendlyChat clients.
  await admin.database().ref('messages').push({
    name: 'Firebase Bot',
    profilePicUrl: '/images/firebase-logo.png', // Firebase logo
    text: `${fullName} signed in for the first time! Welcome!`,
  });
  console.log('Welcome message written to database.');
});
```

Adding this function to the special `exports` object is Node's way of making the function accessible outside of the current file and is required for Cloud Functions.

In the function above we are adding a new welcome message posted by "Firebase Bot" to the list of chat messages. We are doing this by using the [`push`](https://firebase.google.com/docs/database/web/lists-of-data#append_to_a_list_of_data) method on the `/messages` collection in the Firebase Realtime Database which is where the messages of the chat are stored.

Since this is an asynchronous operation, we need to return the [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) indicating when the Realtime Database write has finished, so that Functions doesn't exit the execution too early.

### Deploy the Function

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
⚠  functions: missing necessary APIs. Enabling now...
i  env: ensuring necessary APIs are enabled...
⚠  env: missing necessary APIs. Enabling now...
i  functions: waiting for APIs to activate...
i  env: waiting for APIs to activate...
✔  env: all necessary APIs are enabled
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: creating function addWelcomeMessages...
✔  functions[addWelcomeMessages]: Successful create operation. 
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlypchat-1234/overview
```

Tip: The first time you are deploying functions for a project will take longer than usual because we're enabling APIs on your Google Cloud Project. The length of the deploy also depends on the number of functions being deployed and will increase as you add more over time.

### **Test the function**

Once the function has deployed successfully you'll need to have a user that signs in for the first time.

1. Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2. With a new user, sign in for the first time in your app using the **Sign In** button.

- If you have already signed in the app you can open the [Firebase Console Authentication section](https://console.firebase.google.com/project/_/authentication/users) and delete your account from the list of users. Then sign in again.

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/262535d1b1223c65.png)

1. After you sign in, a welcome message should be displayed automatically:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/2613731a7255735c.png)

Logs: If the welcome message doesn't appear or if you face any other issues further on, you should double-check the command line output for errors during the deploy. If that looks successful, visit the [Cloud Functions logs](https://console.firebase.google.com/project/_/functions/logs?search=&severity=DEBUG) to see if any error messages appeared. You can also verify that the function was deployed successfully and see how many times it has been executed by switching to the `DASHBOARD` tab.





## 9. Images moderation

Users can upload all type of images in the chat, and it is always important to moderate offensive images, especially in public social platforms. In FriendlyChat the images that are being published to the chat are stored into [Google Cloud Storage](https://firebase.google.com/docs/storage/).

With Cloud Functions, you can detect new image uploads using the `functions.storage().onFinalize` trigger. This will run every time a new file is uploaded or modified in Cloud Storage.

To moderate images we'll go through the following process:

1. Check if the image is flagged as Adult or Violent using the [Cloud Vision API](https://cloud.google.com/vision/)
2. If the image has been flagged, download it on the running Functions instance
3. Blur the image using [ImageMagick](https://www.imagemagick.org/)
4. Upload the blurred image to Cloud Storage

### Enable the Cloud Vision API

Since we'll be using the Google Cloud Vision API in this function, you must enable the API on your firebase project. Follow [this link](https://console.cloud.google.com/apis/api/vision.googleapis.com/overview?project=_), select your Firebase project and enable the API:

![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/26681849793bf2e5.png)

Note that you will have to [enable billing](https://console.cloud.google.com/billing?project=_) to enable this API. While most of what you do in this codelab should be within the free tier there is a risk you may be billed depending on your usage. To avoid this, make sure you disable billing at the end of this codelab.

If you do not feel comfortable enabling Billing, feel free to skip this section and move on to New Message Notifications.

### **Install dependencies**

To moderate the images we'll need a few Node.js packages:

- The Google Cloud Vision Client Library for Node.js: [@google-cloud/vision](https://www.npmjs.com/package/@google-cloud/vision) to run the image through the Cloud Vision API to detect inappropriate images.
- A Node.js library allowing us to run processes: [child-process-promise](https://www.npmjs.com/package/child-process-promise) to run ImageMagick since the ImageMagick command-line tool comes pre-installed on all Functions instances.

To install these three packages into your Cloud Functions app, run the following `npm install --save` command. Make sure that you do this from the `functions` directory.

```
npm install --save @google-cloud/vision@0.12.0 child-process-promise@2.2.1
```

This will install the two packages locally and add them as declared dependencies in your `package.json` file.

### **Import and configure dependencies**

To import the two dependencies that were installed and some Node.js core modules (`path`, `os` and `fs`) that we'll need in this section add the following lines to the top of your `index.js` file:

### index.js

```
const Vision = require('@google-cloud/vision');
const vision = new Vision();
const spawn = require('child-process-promise').spawn;

const path = require('path');
const os = require('os');
const fs = require('fs');
```

Since your function will run inside a Google Cloud environment, there is no need to configure the Cloud Storage and Cloud Vision libraries: they will be configured automatically to use your project.

### **Detecting inappropriate images**

You'll be using the `functions.storage.onChange` Cloud Functions trigger which runs your code as soon as a file or folder is created or modified in a Cloud Storage bucket. Add the `blurOffensiveImages` Function into your `index.js` file:

### index.js

```
// Checks if uploaded images are flagged as Adult or Violence and if so blurs them.
exports.blurOffensiveImages = functions.runWith({memory: '2GB'}).storage.object().onFinalize(
    async (object) => {
      const image = {
        source: {imageUri: `gs://${object.bucket}/${object.name}`},
      };

      // Check the image content using the Cloud Vision API.
      const batchAnnotateImagesResponse = await vision.safeSearchDetection(image);
      const safeSearchResult = batchAnnotateImagesResponse[0].safeSearchAnnotation;
      const Likelihood = Vision.types.Likelihood;
      if (Likelihood[safeSearchResult.adult] >= Likelihood.LIKELY ||
          Likelihood[safeSearchResult.violence] >= Likelihood.LIKELY) {
        console.log('The image', object.name, 'has been detected as inappropriate.');
        return blurImage(object.name);
      }
      console.log('The image', object.name, 'has been detected as OK.');
    });
```

Note that we added some configuration of the Cloud Functions instance that will run the function, with `.runWith({memory: '2GB'})` we're requesting that the instance gets 2GB of memory rather than the default, this will help as this function is memory intensive.

When the function is triggered, the image is ran through the Cloud Vision API to detect if it is flagged as adult or violent. If the image is detected as inappropriate based on these criteria we're blurring the image which is done in the `blurImage`function which we'll see next.

### **Blurring the image**

Add the following `blurImage` function in your `index.js` file:

### index.js

```
// Blurs the given image located in the given bucket using ImageMagick.
async function blurImage(filePath) {
  const tempLocalFile = path.join(os.tmpdir(), path.basename(filePath));
  const messageId = filePath.split(path.sep)[1];
  const bucket = admin.storage().bucket();

  // Download file from bucket.
  await bucket.file(filePath).download({destination: tempLocalFile});
  console.log('Image has been downloaded to', tempLocalFile);
  // Blur the image using ImageMagick.
  await spawn('convert', [tempLocalFile, '-channel', 'RGBA', '-blur', '0x24', tempLocalFile]);
  console.log('Image has been blurred');
  // Uploading the Blurred image back into the bucket.
  await bucket.upload(tempLocalFile, {destination: filePath});
  console.log('Blurred image has been uploaded to', filePath);
  // Deleting the local file to free up disk space.
  fs.unlinkSync(tempLocalFile);
  console.log('Deleted local file.');
  // Indicate that the message has been moderated.
  await admin.database().ref(`/messages/${messageId}`).update({moderated: true});
  console.log('Marked the image as moderated in the database.');
}
```

In the above function the image binary is downloaded from Cloud Storage. Then the image is blurred using ImageMagick's `convert` tool and the blurred version is re-uploaded on the Storage Bucket. Then we delete the file on the Cloud Functions instance to free up some disk space, we do this because the same Cloud Functions instance can get re-used and if files are not cleaned up it could run out of disk. Finally we add a boolean to the chat message indicating the image was moderated, this will trigger a refresh of the message on the client.

### **Deploy the Function**

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: updating function addWelcomeMessages...
i  functions: creating function blurOffensiveImages...
✔  functions[addWelcomeMessages]: Successful update operation.
✔  functions[blurOffensiveImages]: Successful create operation.
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```

### **Test the function**

Once the function has deployed successfully:

1. Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2. Once signed-in the app upload an image: ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/43fbd8dd2e559aca.png)
3. Choose your best offensive image to upload (or you can use this [flesh eating Zombie](https://pixabay.com/zombie-flesh-eater-dead-spooky-949916/)!) and after a few moments you should see your post refresh with a blurred version of the image:
   ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/6a2d6c52e6ea89b.png)





## 10. New Message Notifications

In this section you will add a Cloud Function that sends notifications to participants of the chat when a new message is posted.



> SM - Not Working (See FriendlyChat codelab - > how is this implemented...)
>
> ```bash
> Failure sending notification to cTbSeTWFbBU:APA91bFoRJgAvbaFC1xd_pvjFPFUrbBuq2FuAYv-eBGgECOXc2kNb6DqfByIHw7OIge0DSN1kPTUo7n23g3Mo52Cr_NQYWZqRWFUl5YhhvdRVIaXyDGwJRVzRdMrIqrlNULYWNlu0vdG { Error: The provided registration token is not registered. A previously valid registration token can be unregistered for a variety of reasons. See the error documentation for more details. Remove this registration token and stop using it to send messages.
>     at FirebaseMessagingError.FirebaseError [as constructor] (/srv/node_modules/firebase-admin/lib/utils/error.js:39:28)
>     at FirebaseMessagingError.PrefixedFirebaseError [as constructor] (/srv/node_modules/firebase-admin/lib/utils/error.js:85:28)
>     at new FirebaseMessagingError (/srv/node_modules/firebase-admin/lib/utils/error.js:241:16)
>     at Function.FirebaseMessagingError.fromServerError (/srv/node_modules/firebase-admin/lib/utils/error.js:271:16)
>     at /srv/node_modules/firebase-admin/lib/messaging/messaging.js:342:63
>     at Array.forEach (<anonymous>)
>     at mapRawResponseToDevicesResponse (/srv/node_modules/firebase-admin/lib/messaging/messaging.js:338:26)
>     at /srv/node_modules/firebase-admin/lib/messaging/messaging.js:519:24
>     at <anonymous>
>     at process._tickDomainCallback (internal/process/next_tick.js:228:7)
>   errorInfo: 
>    { code: 'messaging/registration-token-not-registered',
>      message: 'The provided registration token is not registered. A previously valid registration token can be unregistered for a variety of reasons. See the error documentation for more details. Remove this registration token and stop using it to send messages.' },
>   codePrefix: 'messaging' }
> ```











Using [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) you can send notifications to your users in a cross platform and reliable way. To send a notification to a user you need their FCM device token. The chat web app that we are using already collects device tokens from users when they open the app for the first time on a new browser or device. These tokens are stored in the Firebase Realtime Database under the `/fcmTokens/$deviceToken` path.

If you would like to learn how to get FCM device tokens on a web app you can go through the [Firebase Web Codelab](http://codelabs.developers.google.com/codelabs/firebase-web/).

### **Send notifications**

To detect when new messages are posted you'll be using the `functions.database.ref().onCreate` Cloud Functions trigger which runs your code when a new object is created at a given path of the Firebase Realtime Database. Add the `sendNotifications` function into your `index.js` file:

### index.js

```
// Sends a notifications to all users when a new message is posted.
exports.sendNotifications = functions.database.ref('/messages/{messageId}').onCreate(
    async (snapshot) => {
      // Notification details.
      const text = snapshot.val().text;
      const payload = {
        notification: {
          title: `${snapshot.val().name} posted ${text ? 'a message' : 'an image'}`,
          body: text ? (text.length <= 100 ? text : text.substring(0, 97) + '...') : '',
          icon: snapshot.val().photoUrl || '/images/profile_placeholder.png',
          click_action: `https://${process.env.GCLOUD_PROJECT}.firebaseapp.com`,
        }
      };

      // Get the list of device tokens.
      const allTokens = await admin.database().ref('fcmTokens').once('value');
      if (allTokens.exists()) {
        // Listing all device tokens to send a notification to.
        const tokens = Object.keys(allTokens.val());

        // Send notifications to all tokens.
        const response = await admin.messaging().sendToDevice(tokens, payload);
        await cleanupTokens(response, tokens);
        console.log('Notifications have been sent and tokens cleaned up.');
      }
    });
```

In the Function above we are gathering all users' device tokens from the Firebase Realtime Database and sending a notification to each of these using the `admin.messaging().sendToDevice` function.

### **Cleanup the tokens**

Lastly we want to remove the tokens that are not valid anymore. This happens when the token that we once got from the user is not being used by the browser or device anymore. For instance, this happens if the user has revoked the notification permission for his browser session. To do this add the following `cleanupTokens` function in your `index.js` file:

### index.js

```
// Cleans up the tokens that are no longer valid.
function cleanupTokens(response, tokens) {
 // For each notification we check if there was an error.
 const tokensToRemove = {};
 response.results.forEach((result, index) => {
   const error = result.error;
   if (error) {
     console.error('Failure sending notification to', tokens[index], error);
     // Cleanup the tokens who are not registered anymore.
     if (error.code === 'messaging/invalid-registration-token' ||
         error.code === 'messaging/registration-token-not-registered') {
       tokensToRemove[`/fcmTokens/${tokens[index]}`] = null;
     }
   }
 });
 return admin.database().ref().update(tokensToRemove);
}
```

### **Deploy the Function**

The Function will only be active after you've deployed it. On the command line run `firebase deploy --only functions`:

```
firebase deploy --only functions
```

This is the console output you should see:

```
i  deploying functions
i  functions: ensuring necessary APIs are enabled...
✔  functions: all necessary APIs are enabled
i  functions: preparing functions directory for uploading...
i  functions: packaged functions (X.XX KB) for uploading
✔  functions: functions folder uploaded successfully
i  starting release process (may take several minutes)...
i  functions: updating function addWelcomeMessages...
i  functions: updating function blurOffensiveImages...
i  functions: creating function sendNotifications...
✔  functions[addWelcomeMessages]: Successful update operation.
✔  functions[blurOffensiveImages]: Successful updating operation.
✔  functions[sendNotifications]: Successful create operation.
✔  functions: all functions deployed successfully!

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/friendlychat-1234/overview
```

### Test the function

Once the function has deployed successfully you'll need to have a user that signs-in for the first time. If you have signed-in already with your account you can open the Firebase Console Authentication section and delete your account from the list of users.

1. Open your app in your browser using the hosting URL (in the form of `https://<project-id>.firebaseapp.com`).
2. If you sign-in the app for the first time make sure you allow notifications when prompted:
   ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/93ec8bf8a1fea7e1.png)
3. Close the chat app tab or display a different tab: Notifications appear only if the app is in the background. If you would like to learn how to receive messages while your app is in the foreground have a look at [our documentation](https://firebase.google.com/docs/cloud-messaging/js/receive#handle_messages_when_your_web_app_is_in_the_foreground).
4. Using a different browser (or an Incognito window), sign into the app and post a message. You should see a notification displayed by the first browser:
   ![img](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/img/1e9ca7256d5508b9.png)

Note: if you attempt to adapt this codelab to reach a third-party service outside of Google, you need to switch to the a paid plan. See the [pricing page](https://firebase.google.com/pricing/) for more details.



## 11. Congratulations!

You have used the Firebase SDK for Cloud Functions and added server-side components to a chat app.

### **What we've covered**

- Authoring Cloud Functions using the Firebase SDK for Cloud Functions.
- Trigger Cloud Functions based on Auth, Cloud Storage and Realtime Database events.
- Add Firebase Cloud Messaging support to your web app.
- Deployed Cloud Functions using the Firebase CLI.

### Next Steps

- Learn about the [other Cloud Function trigger types](https://firebase.google.com/docs/functions/firestore-events).
- Use Firebase and Cloud Functions with your own app.