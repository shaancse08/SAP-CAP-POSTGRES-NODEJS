# Prepare CAP Project

To start the CAP project we have used the traditional command

```sh
cds init sap-cap-nodejs-postgres
```

Once project initialized we have used 
```sh
cds add tiny-sample
```
command to add a sample project to our project. So that we can start our project.

Then we have used the below command to deploy the data into local Sqlite and start the server.

```sh
cds deploy --to sqlite:requirs.db
cds watch
```

Once don then we will see our project running at http://localhost:4004
