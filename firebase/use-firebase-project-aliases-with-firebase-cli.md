# Use Firebase project aliases with Firebase CLI (multiple Firebase projects)

1. Goal
2. Setup

**Links:**<br/>
https://firebase.google.com/docs/cli/#add_alias<br/>
https://victorbruce82.medium.com/how-to-deploy-a-react-app-to-different-firebase-hosting-environments-dev-and-prod-da3f4cae9a1e

## 1. Goal
Configure Firebase CLI to access/manage multiple Firebase projects via `-P` parameter

Example:<br/>
Run `firebase deploy -P dev` to deploy to a _development_ Firebase Project<br/>
Run `firebase deploy -P prod` to deploy to a _production_ Firebase Project


## 2. Setup

1. Run `firebase use --add`
2. Select your _development_ Firebase Project
3. Set `dev` as alias
4. Run `firebase use --add` (again)
5. Select your _production_ Firebase Project
6. Set `prod` as alias

A `.firebaserc` file will be generated and should look something like this:
```
{
  "projects": {
    "default": "DEV_PROJECT_ID",
    "dev": "DEV_PROJECT_ID",
    "prod": "PROD_PROJECT_ID"
  },
  "targets": {},
  "etags": {}
}
```
You can edit this file at any time.

You should (also) select your _development_ project as the `default` project. If you don't specify the `-P` parameter in any Firebase CLI command, the `default` project will be taken.

Example:<br/>
`firebase deploy` => uses `default` project<br/>
`firebase deploy -P dev` => uses `dev` project<br/>
`firebase deploy -P prod` => uses `prod` project
