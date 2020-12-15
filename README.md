# Firebase Cheatsheet
A cheatsheet for using firebase

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

# Firestore

import 'firebase/firestore';

### Base Reference

    let ref = firebase.firestore().collection(collectionName)

### Specifing a document

    let doc = firebase.firestore().collection(collectionName).doc(docId)

### CRUD operations

Get the data of a document

    let getData = await ref.doc(docId).get();
    let data = getData.exists ? getData.data() : null;

Get all the data of a collection

    let allData = await ref.get();
    allData.forEach(doc => // doc.id & doc.data());

Setting, updating and deleting data

    await ref.doc(docId).set(data);
    
    await ref.doc(docId).update(updatedData);
    
    await ref.doc(docId).delete();

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

