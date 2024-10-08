# Unity Catalog Server

This is the open source version of the Unity Catalog server implementation. It can be started locally for local usage (e.g. for testing) without any Databricks account.

## Running the Server

The server can be started by issuing the below command from the project root directory:

```sh
bin/start-uc-server
```

## Configuration

The server config file is at the location `etc/conf/server.properties` (relative to the project root).

- `server.env`: The environment in which the server is running. This can be set to `dev` or `test`. When set to `test` the server will instantiate an empty in-memory h2 database for storing metadata. If set to `dev`, the server will use the file `etc/db/h2db.mv.db` as the metadata store. Any changes made to the metadata will be persisted in this file.

For enabling server to vend AWS temporary credentials to access S3 buckets (for accessing External tables/volumes),
the following parameters need to be set:

- `s3.bucketPath.i`: The S3 path of the bucket where the data is stored. Should be in the format `s3://<bucket-name>`.
- `s3.accessKey.i`: The AWS access key, an identifier of temp credentials.
- `s3.secretKey.i`: The AWS secret key used to sign API requests to AWS.
- `s3.sessionToken.i`: THE AWS session token, used to verify that the request is coming from a trusted source.

You can configure multiple buckets by incrementing the index <i>i</i> in the above parameters. The starting index should be 0.

All the above parameters are required for each index. For vending temporary credentials, the server matches the bucket path in the table/volume storage_location with the bucket path in the configuration and returns the corresponding access key, secret key, and session token.

Any params that are not required can be left empty.

### Logging

The server logs are located at `etc/logs/server.log`. The log level and log rolling policy can be set in log4j2 config file: `etc/conf/server.log4j2.properties`
