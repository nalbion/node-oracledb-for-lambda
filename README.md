# node-oracledb-for-lambda

This module is a fork of [node-oracledb](https://github.com/oracle/node-oracledb) v1.9.3, precompiled for AWS Lambda, Node.js 4.3.

The scripts to reproduce the build process can be found at [node-oracledb-lambda-test](https://github.com/nalbion/node-oracledb-lambda-test). 

# Usage

In addition to the usual package.json and *.js files, you also need to include the 
Oracle Instant Client libraries provided in `lambda-lib` - they should be in your zip file's `lib/` directory.

```bash
npm install --save oracledb-for-lambda

zip app.zip index.js package.json \
   your-other-dependencies... \
   node_modules/oracledb-for-lambda/package.json \
   node_modules/oracledb-for-lambda/index.js \
   node_modules/oracledb-for-lambda/lib/*.js \
   node_modules/oracledb-for-lambda/build/Release/oracledb.node \
   lib/*.so* \
```

Note: There is an [issue with `gulp-zip` modifying the structure of these binaries](https://github.com/thejoshwolfe/yazl/issues/25), so just use the native `zip`. 


## Instant Client Light
To keep the zip and application size small, the [Instant Client Light (English) version](https://docs.oracle.com/database/121/LNOCI/oci01int.htm#LNOCI13309) is provided by default.
If you require error messages in languages other than English, you can swap the Instant Client Data library:

```bash
# after npm install, and before creating your zip:
cp node_modules/oracle-for-lambda/lib/libociei.so lib/
rm lib/libociicus.so
```

Due to the size of the Oracle libraries, you may need to deploy your zip file to S3 and get Lambda to download from the S3 URL.
