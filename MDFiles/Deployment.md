# Deployment

When deploying to Cloud Foundry, this can be accomplished by providing a simple deployer app, which you can construct as follows:

- Create a new folder named `gen/pg/db`

  ```sh
  mkdir -p gen/pg/db
  ```

- Generate a precompiled cds model:

  ```sh
  cds compile '*' > gen/pg/db/csn.json
  ```

- Add required `.csv` files, for example:

  ```sh
  cp -r db/data gen/pg/db/data
  ```

- Add a `package.json` to `gen/pg` with this content:
  ```json
  {
    "engines": {
      "node": "^18"
    },
    "dependencies": {
      "@sap/cds": "*",
      "@cap-js/postgres": "*"
    },
    "scripts": {
      "start": "cds-deploy"
    }
  }
  ```
- Add `mta` file in the project

  ```sh
  cds add mta
  ```

- Add `PostgreSQL, Hyperscaler Option` in BTP. So that we can use it in our resources.

  ```yaml
  resources:
    - name: sap-sample-postgres-db
    type: org.cloudfoundry.managed-service
    parameters:
        service: postgresql-db
        service-plan: trial
  ```

- Change the `mta` service module according to the resource.

  ```yaml
  modules:
    - name: sap-cap-nodejs-postgres-srv
    type: nodejs
    path: gen/srv
    parameters:
        buildpack: nodejs_buildpack
    build-parameters:
        builder: npm
    provides:
        - name: srv-api
        properties:
            srv-url: ${default-url}
    requires:
        - name: sap-sample-postgres-db
  ```

- Add the DB deployer in the `mta` file.

  ```yaml
    - name: sap-sample-postgres-db-deployer
    type: nodejs
    path: gen/pg
    parameters:
        no-route: true
        no-start: true
        disk-quota: 1GB
        memory: 256MB
        tasks:
        - name: deploy-to-postgresql
            command: npm start
            disk-quota: 1GB
            memory: 256MB
    build-parameters:
        ignore: ["node_modules/"]
    requires:
        - name: sap-sample-postgres-db
  ```

- Add `Passport` and `@sap/xssec` dependency to the project.

  ```sh
  npm add passport
  npm add @sap/xssec
  ```

- Add `pg-build.sh` file in the root folder with the below command. Because which ever commands we have done in the first four steps. Those will be over written by the `MBT Build` command so that's why we are using the `.sh` file to execute the shell commands after `cds build` command.

  ```sh
  #!/bin/bash
  mkdir -p gen/pg/db
  # Works only the first time until https://github.com/cap-js/cds-dbs/issues/100 is fixed
  # cp -r db/data gen/pg/db
  cds compile '*' > gen/pg/db/csn.json
  cp pg-package.json gen/pg/package.json
  cp package-lock.json gen/pg/package-lock.json
  ```

- Add the build commands in MTA like below.

  ```yaml
  build-parameters:
  before-all:
      - builder: custom
      commands:
          - npx cds build
          - ./pg-build.sh
  ```

- Now build the project using
  ```sh
  mbt build
  ```


If the Above process doesn't work then we will follow the next option to deploy the application.
---


- Add `mta` file in the project

  ```sh
  cds add mta
  ```

- Add `PostgreSQL, Hyperscaler Option` in BTP. So that we can use it in our resources.

  ```yaml
  resources:
    - name: sap-sample-postgres-db
    type: org.cloudfoundry.managed-service
    parameters:
        service: postgresql-db
        service-plan: trial
  ```

- Change the `mta` service module according to the resource.

  ```yaml
  modules:
    - name: sap-cap-nodejs-postgres-srv
    type: nodejs
    path: gen/srv
    parameters:
        buildpack: nodejs_buildpack
    build-parameters:
        builder: npm
    provides:
        - name: srv-api
        properties:
            srv-url: ${default-url}
    requires:
        - name: sap-sample-postgres-db
  ```

- Add the DB deployer in the `mta` file.

  ```yaml
    - name: sap-sample-postgres-db-deployer
    type: nodejs
    path: gen/pg
    parameters:
        no-route: true
        no-start: true
        disk-quota: 1GB
        memory: 256MB
        tasks:
        - name: deploy-to-postgresql
            command: npm start
            disk-quota: 1GB
            memory: 256MB
    build-parameters:
        ignore: ["node_modules/"]
    requires:
        - name: sap-sample-postgres-db
  ```

- Add `Passport` and `@sap/xssec` dependency to the project.

  ```sh
  npm add passport
  npm add @sap/xssec
  ```

- Remove the build commands from MTA like below.

  ```yaml
  build-parameters:
  before-all:
      - builder: custom
      commands:
          - npx cds build
          - ./pg-build.sh
  ```

- Execute `npx cds build` command

- Create a new folder named `gen/pg/db`

  ```sh
  mkdir -p gen/pg/db
  ```

- Generate a precompiled cds model:

  ```sh
  cds compile '*' > gen/pg/db/csn.json
  ```

- Add required `.csv` files, for example:

  ```sh
  cp -r db/data gen/pg/db/data
  ```

- Add a `package.json` to `gen/pg` with this content:
  ```json
  {
    "engines": {
      "node": "^18"
    },
    "dependencies": {
      "@sap/cds": "*",
      "@cap-js/postgres": "*"
    },
    "scripts": {
      "start": "cds-deploy"
    }
  }
  ```  

- Now build the project using
  ```sh
  mbt build
  ```

