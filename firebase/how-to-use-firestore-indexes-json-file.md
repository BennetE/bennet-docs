# How to use `firestore.indexes.json` file

1. Setup
2. Generate index via error message
3. Get indexes via CLI
4. Deploy indexes file

**Links:**<br/>
https://firebase.google.com/docs/firestore/query-data/indexing#use_the_firebase_cli<br/>
https://firebase.google.com/docs/reference/firestore/indexes/<br/>
https://firebase.google.com/docs/cli#partial_deploys<br/>
[Why you sometimes need to create indexes?](https://firebase.google.com/docs/firestore/query-data/indexing)

## 1. Setup

1. It is assumed that 2 firebase projects were created, one for testing and the other for production
2. Steps 2 and 3 are executed in the test/dev firebase project

## 2. Generate index via error message

1. Run a firestore query that needs a missing index
2. An error message with a link will appear in the browser console
3. Follow the link, this will direct you to the Firebase console
4. Follow the instructions and create the index
5. It is not necessary to wait until the generated indexes have finished loading in the firebase console to proceed with step 3

## 3. Get indexes via CLI

1. Run `firebase firestore:indexes` in you project
    - If you use [project aliases](https://github.com/BennetE/bennet-docs/blob/main/firebase/use-firebase-project-aliases-with-firebase-cli.md) use `firebase firestore:indexes -P <YOUR_ALIAS>`
2. Copy the output to the `firestore.indexes.json` file

## 4. Deploy indexes file

1. Run the command `firebase deploy --only firestore:indexes` to deploy the indexes
2. If you have a production firebase project, you can use [project aliases](https://github.com/BennetE/bennet-docs/blob/main/firebase/use-firebase-project-aliases-with-firebase-cli.md) to publish the indexes to this project as well
