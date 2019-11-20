# firebase-ci

> Testing GitHub actions for CI to Firebase Hosting.


The following tutorial discusses how to use GitHub Actions to deploy a basic static web project to Firebase Hosting.


### Content

- Prerequisites
- Steps
- Forking a Firebase Hosting Web Project with Included Actions



## Prerequisites

1. **NodeJS**
   - version 10.16.3 was used for this tutorial

2. **Firebase CLI**  
   - `npm install -g firebase-tools`
   - firebase cli version `7.6.2` was used for this tutorial

3. **GitHub account**

4. **Google Account**
   - A Firebase Hosting web project properly set-up from the online [Firebase Console](https://console.firebase.google.com/).

5. The Firebase Hosting **project files** (with references from # 4) committed in the GitHub account's repository's @master branch.
   - a default Hosting project can be set-up from the **firebase cli**
      `firebase init`, choosing only the `Hosting` option.



## Steps

1. Get your Firebase project's token from the firebase cli  
`firebase login:ci`

2. Add the token to your GitHub project's **Secrets**
   - Go to project repository's **Settings** -> **Secrets**
   - Press the **[Add a new secret]** button
      - Enter a name for your token, i.e.: `FIREBASE_API_TOKEN`
      - Paste the value you have copied from **step # 1**

3. Go to your GitHub project's **Actions** page. This can be accessed from the **Actions** tab, or go to: 
   - https://github.com/<GITHUB_ACCNT>/PROJECT_NAME/actions/new

4. Press the **Set up a workflow yourself** button.

5. Paste the following value to the **.yml** file which you will create.

 
		name: Deploy to Firebase Hosting
		
		on:
		  push:
		    branches: 
		      - master
		
		jobs:
		  deploy:
		
		    runs-on: ubuntu-latest
		
		    steps:
		      - name: Checkout the repository
		        uses: actions/checkout@v1
		        
		      - name: Deploy to Firebase
		        uses: w9jds/firebase-action@master
		        with:
		          args: deploy --only hosting
		        env:
		          FIREBASE_TOKEN : ${{ secrets.FIREBASE_API_TOKEN }}


6. Press the **Start Commit** button.
   - the yml file will be saved and pushed to the /master branch at  
   `.github/workflows/main.yml`
   - Deployment to Firebase Hosting will automatically start
   - Logs can be viewed from the project's [**[Actions]**](https://github.com/weaponsforge/firebase-ci/actions) tab.

7. Refer to the [**GitHub Action for Firebase**](https://github.com/marketplace/actions/github-action-for-firebase) for a list of other firebase deploy and build options that you can use in the **.yml** file.



## Forking a Firebase Hosting Web Project with Included Actions

GitHub Actions are automatically disabled when a GitHub project with Actions is forked. This is to give a chance to the developer to set-up requirements (specially for deployment, etc) first.

Update the following after Forking this repository:

1.  Refer to the **Prerequisites # 4 - Google Account** - Firebase project.
   - Open the `.firebaserc` file. Rename the firebase project name listed here to match your firebase project name.

2. Go over the **Steps** section no.'s **# 1, # 2,** and **# 3** to set up the firebase token to your forked GitHub project.

3. Go the the forked repository's **Actions** tab and press the **[I understand my workflows, go ahead and run them]** button to start deployment to *your* Firebase Hosting project.


20191120