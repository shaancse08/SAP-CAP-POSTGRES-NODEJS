# Getting Started With Postgres With SAP CAPM 

The domain model in CAP (Cloud Application Programming) is crucial for defining domain entities using CDS (Core Data Services), allowing seamless integration with external services or databases. CAP, along with its associated tools, automates the translation of CDS models into database-supported schemas. CAP provides native support for various databases such as SAP HANA (Cloud), PostgreSQL, SQLite, and H2. To learn more about CAP’s database support, please refer to [CAP – Database Support](https://cap.cloud.sap/docs/about/features#database-support).

For Node.js, before we use the CAP-community-provided adapter cds-pg in combination with cds-dbm to consume PostgreSQL. Since cds 7, CAP Node.js has natively supported PostgreSQL by releasing new database services and its implementation [@cap-js/postgres](https://www.npmjs.com/package/@cap-js/postgres). [@cap-js/postgres](https://www.npmjs.com/package/@cap-js/postgres) provides the functionalities to translate the incoming requests from CDS model to PostgreSQL during runtime, and analyze the delta between the current state of the database and the current state of the CDS model, deploy the changes to the database, load CSV files, etc.

- ### [Prepare CAP Project](./MDFiles/PrepareCAPProject.md)
- ### [Connect to PostgreSQL Locally](./MDFiles/ConnectToPostgreSQLLocally.md)
