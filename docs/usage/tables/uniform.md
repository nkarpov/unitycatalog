# Unity Catalog with UniForm tables

## Read Delta Uniform tables via Iceberg REST Catalog

Delta Tables with Uniform enabled can be accessed via Iceberg REST Catalog. The Iceberg REST Catalog is served at
`http://127.0.0.1:8080/api/2.1/unity-catalog/iceberg/`.

A pre-populated Delta Uniform table can be prepared by running `cp -r etc/data/external/unity/default/tables/marksheet_uniform /tmp/marksheet_uniform`.

### Setting up REST Catalog with Apache Spark

The following is an example of the settings to configure OSS Apache Spark to read UniForm as Iceberg:

```
"spark.sql.extensions": "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions",
"spark.sql.catalog.iceberg": "org.apache.iceberg.spark.SparkCatalog",
"spark.sql.catalog.iceberg.catalog-impl": "org.apache.iceberg.rest.RESTCatalog",
"spark.sql.catalog.iceberg.uri": "http://127.0.0.1:8080/api/2.1/unity-catalog/iceberg",
"spark.sql.catalog.iceberg.token": "not_used",
```

When querying Iceberg REST Catalog for Unity Catalog, tables are identified using the following pattern `iceberg.<catalog-name>.<schema-name>.<table-name>` (e.g. `iceberg.unity.default.marksheet_uniform`).