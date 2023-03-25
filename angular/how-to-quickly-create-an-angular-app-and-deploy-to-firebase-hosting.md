# How to quickly create an angular app and deploy to firebase hosting

create angular app
```
ng new my-app
cd my-app
```


create firebase project
```
npm install -g firebase-tools
firebase login
firebase projects:create your-project-id --display-name "Your Project Name"
firebase projects:list
```

hosting 
```
firebase init hosting

? Are you ready to proceed? (Y/n)
y

? Please select an option: (Use arrow keys)
> Use an existing project
  Create a new project
  Add Firebase to an existing Google Cloud Platform project
  Don't set up a default project
  
  Use an existing project
  
? Please select an option: Use an existing project
? Select a default Firebase project for this directory:

? What do you want to use as your public directory? 
dist

? Configure as a single-page app (rewrite all urls to /index.html)? (y/N) 
y

? Set up automatic builds and deploys with GitHub? (y/N)
n

```


modify angular.json
```
{

...

        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/my-app", -> change to "dist/"
            "index": "src/index.html",


...

}

```

build app
```
ng build --configuration production
```



```
firebase deploy
```






  
  
