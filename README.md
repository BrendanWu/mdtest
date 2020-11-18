# Continuous data backup

In the event of a "disaster" shut down production database to stop write requests to the database. Contact system admin

- App takes periodic snapshots of production database (cloud backup).

[Database Snapshot Schedules (MongoDB Atlas) ](https://www.notion.so/c98edbd048c04fb9afa83c0dfe3b62f5)

By default, Atlas takes base snapshots every 6 hours.

[Untitled](https://www.notion.so/86c7d4c5b68f4337927c40899ed3b615)

- It is also best practices to download local hard copies via Atlas service.

**LIMITATIONS OF CLOUD BACKUP**

Cloud Backups:

- Can support sharded clusters running MongoDB version 3.6 or later.
- Cannot guarantee causal consistency of your data.
- Cannot restore an existing snapshot to a cluster after you add or remove a shard from it. You may restore an existing snapshot to another cluster with a matching topology.
- Cannot take a consistent snapshot of a cluster if [chunks are manually migrated](https://docs.mongodb.com/manual/tutorial/migrate-chunks-in-sharded-cluster) while the snapshot is being taken.

# Import Data into a Collection

MongoDB Compass can import data into a collection from either a **JSON** or **CSV** file.

### **Limitations**

- Importing data into a collection is not permitted in **MongoDB Compass Readonly Edition**.
- Importing data is not available if you are connected to a [Data Lake](https://docs.atlas.mongodb.com/data-lake).

### **Format Your Data**

Before you can import your data into MongoDB Compass you must first ensure that it is formatted correctly.

**JSONCSV**

When importing data from a **JSON** file, you can format your data as:

- Newline-delimited documents, or
- Comma-separated documents in an array

**EXAMPLE**

The following newline-delimited `.json` file is formatted correctly:

```
{ "type": "home", "number": "212-555-1234" }{ "type": "cell", "number": "646-555-4567" }{ "type": "office", "number": "202-555-0182"}
```

The following comma-separated `.json` array file is also formatted correctly:

```
[{ "type": "home", "number": "212-555-1234" }, { "type": "cell", "number": "646-555-4567" }, { "type": "office", "number": "202-555-0182"}]
```

Compass ignores line breaks in JSON arrays.

Compass automatically generates [ObjectIDs](https://docs.mongodb.com/manual/reference/method/ObjectId/) for these objects on import since no ObjectIDs were specified in the initial JSON.

### **Procedure**

To import your formatted data into a collection:

**1**

### **Connect to the deployment containing the [collection](https://docs.mongodb.com/compass/master/collections) you wish to import data into.**

To learn how to connect to a deployment, see [Connect to MongoDB](https://docs.mongodb.com/compass/master/connect#std-label-connect-run-compass).

**2**

### **Navigate to your desired collection.**

You can either select the collection from the [Collections](https://docs.mongodb.com/compass/master/collections) tab or click the collection in the left-hand pane.

**3**

### **Click the *Add Data* dropdown and select *Import File*.**

![https://docs.mongodb.com/compass/master/images/compass/add-data-button.png](https://docs.mongodb.com/compass/master/images/compass/add-data-button.png)

Compass displays the following dialog:

![https://docs.mongodb.com/compass/master/images/compass/import-data-dialog.png](https://docs.mongodb.com/compass/master/images/compass/import-data-dialog.png)

**4**

**Select the location of the source data file under *Select File*.**

**5**

### **Choose the appropriate file type.**

Under **Select Input File Type**, select either **JSON** or **CSV**.

If you are importing a CSV file, you may specify fields to import and the types of those fields under **Specify Fields and Types**. The default data type for all fields is string.

![https://docs.mongodb.com/compass/master/images/compass/import-csv-options.png](https://docs.mongodb.com/compass/master/images/compass/import-csv-options.png)

To exclude a field from a CSV file you are importing, uncheck the checkbox next to that field name. To select a type for a field, use the dropdown menu below that field name.

**6**

### **Configure import options.**

Under **Options**, configure the import options for your use case.

If you are importing a CSV file, you may select how your data is delimited.

For both JSON and CSV file imports, you can toggle **Ignore empty strings** and **Stop on errors**:

- If checked, **Ignore empty strings** drops fields with empty string values from your imported documents. The document is still imported with all other fields.
- If checked, **Stop on errors** prevents any data from being imported in the event of an error. If unchecked, data is inserted until an error is encountered and successful inserts are not rolled back. The import operation will not continue after encountering an error in either case.

**7**

**Click *Import*.**

A progress bar displays the status of the import. If an error occurs during import, the progress bar turns red and an error message appears in the dialog. After successful import, the dialog closes and Compass displays the collection page containing the newly imported documents.

# Export Data from a Collection

MongoDB Compass can export data from a collection as either a **JSON** or **CSV** file. If you specify a [filter](https://docs.mongodb.com/compass/master/query/filter#std-label-query-bar-filter), Compass only exports documents which match the specified query.

### **Behavior**

While it is possible to exclude documents by using a query filter, it is not possible to re-shape exported documents with a [project](https://docs.mongodb.com/compass/master/query/project#std-label-query-bar-project) document. Even when you specify a `project` option in the query, Compass still exports the entire document.

### **Procedure**

**Export Entire CollectionExport Filtered Subset of a Collection**

To export an entire collection to a file:

**1**

### **Connect to the deployment containing the [collection](https://docs.mongodb.com/compass/master/collections) you wish to export data from.**

To learn how to connect to a deployment, see [Connect to MongoDB](https://docs.mongodb.com/compass/master/connect#std-label-connect-run-compass).

**2**

### **Navigate to your desired collection.**

You can either select the collection from the [Collections](https://docs.mongodb.com/compass/master/collections) tab or click the collection in the left-hand pane.

**3**

### **Click *Collection* in the top-level menu and select *Export Collection*.**

![https://docs.mongodb.com/compass/master/images/compass/export-data-option-select.png](https://docs.mongodb.com/compass/master/images/compass/export-data-option-select.png)

Compass displays the following dialog:

![https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-1.png](https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-1.png)

The export dialog initially displays the query entered in the query bar prior to export, if applicable. If no query was specified, this section displays a `find` operation with no parameters, which returns all documents in the collection.

To ignore the query filter and export your entire collection, select **Export Full Collection** and click **Select Fields**.

**4**

### **Select document fields to include in your exported file.**

Only fields that are checked are included in the exported file.

You can add document fields to include with the **Add Field** button if the field you want to include is not automatically detected.

![https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-2.png](https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-2.png)

**5**

### **Choose a file type and export location.**

Under **Select Export File Type**, select either **JSON** or **CSV**. If you select **JSON**, your data is exported to the target file as a comma-separated array.

Then, under **Output**, choose where to export the file to.

![https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-3.png](https://docs.mongodb.com/compass/master/images/compass/export-data-dialog-3.png)

**6**

**Click *Export*.**

A progress bar displays the status of the export. If an error occurs during export, the progress bar turns red and an error message appears in the dialog. After successful export, the dialog closes.

# Import and Export Data from the Command Line

To import and export data from the command line, you can use MongoDB's [Database Tools](https://docs.mongodb.com/database-tools/). See [mongoimport](https://docs.mongodb.com/database-tools/mongoimport) and [mongoexport](https://docs.mongodb.com/database-tools/mongoexport).
