# Firebase Cheatsheet
A cheatsheet for using firebase

### Table of Contents

* [Authentication](#authentication)
* [Security Rules](#security-rules)
* [Firestore](#firestore)
* [Realtime Database](#realtime-database)
* [Functions](#functions)
* [Cloud Storage](#cloud-storage)
* [Hosting](#hosting)


# Notes

* Avoiding Vendor lockin by creating wrapper function for every firebase function used

* Use Firestore for more when you have more complex data to store and have to make less requests (< 50K requests)

* Use Realtime Database when you have less data and have to make a lot of frequent requests (> 50K requests)

* Copy your Firebase web app's Firebase configuration and paste in a global JS file or your index.html file


# Authentication

import 'firebase/auth';

There are 5 main auth functions:

### Firebase Create User

With email and password
    
    await firebase.auth().createUserWithEmailAndPassword(email, password);

### Firebase Signin

    await firebase.auth().signInWithEmailAndPassword(email, password);

### Firebase Signout

    await firebase.auth().signOut()

### Firebase Auth Listener

    let unsubscribe = firebase.auth().onAuthStateChanged(callback);
    
    // unsubscribe function clears the listener and callback function takes a user parameter

### Firebase Current User

    firebase.auth().currentUser;
    // return user or null

# Security Rules

Security Rules controls who can and can't read and write data into firebase databases and storage.

### Cloud Firestore and Cloud Storage syntax

    rules_version = '2'; // Latest version here
    service <<name>> {
        match <<path>> {
            allow <<methods>> : if <<condition>>
        }
    }

### Cloud Firestore example

    rules_version = '2';
    service cloud.firestore {
        match /database/{database}/documents {
            match /{document=**}{
                allow write : if false;
                allow read: if request.auth.uid !== null;
            }

            match /users/{userId} {
                allow write: if request.auth.uid == userId;
            }
        }
    }


### Realtime Database syntax

    {
        "rules": {
            "<<path>>": {
                ".read": <<condition>>,
                ".write": <<condition>>,
                ".validate": <<condition>>
                "<<path>>": {
                    "<<path>>": {
                        ".read": <<condition>>,
                        ".write": <<condition>>
                    }
                }
            }
        }
    }

### Realtime Database example

    {
        "rules": {
            ".read": "true",
            ".write": "auth!=null",
            "posts": {
                "$userId": {
                    "$postId": {
                        ".write": true
                    }
                }
            } 
        }
    }

Learn the Firebase Security Rules Language [here](https://firebase.google.com/docs/rules/rules-language).

# Firestore

import 'firebase/firestore';

### Collection Reference

    let ref = firebase.firestore().collection(collectionName)

### Specifing a document

    let doc = firebase.firestore().collection(collectionName).doc(docId)

### Getting Firestore data

Get the data of a document

    let getData = await ref.doc(docId).get();
    let data = getData.exists ? getData.data() : null;

Get all the data of a collection

    let allData = await ref.get();
    allData.forEach(doc => // doc.id & doc.data());

### Setting Firestore data

Adding data to a collection

    await ref.add(data);

Setting data to a document
    
    await ref.doc(docId).set(data);

### Updating Firestore data

Updating collection data
    
    await ref.doc(docId).update(updatedData);

### Deleting Firestore data

Deleting a document

    await ref.doc(docId).delete();

Deleting a field

    let removefield = ref.update({
        fieldName: firebase.firestore.FieldValue.delete()
    });


### Conditions

    let queryData = await ref.where('key', 'condition', 'value').get();

    queryData.forEach(doc => // doc.id & doc.data());

### Ordering and Limiting

    await ref.orderBy("field", "desc").limit(num).get();

### Get realtime updates

Realtime updates of a collection

    ref.onSnapshot(results => {
        results.map/foreach(doc => // doc.id & doc.data())
    });

Realtime updates of a document

    doc.onSnapshot(doc => // doc.id & doc.data());

### Transactions and Batches

    // ...


# Realtime Database



# Functions

### Setup

Type 'firebase init' and select 'Functions' and follow the required steps in the terminal

To deploy use 'firebase deploy'


# Cloud Storage


# Hosting