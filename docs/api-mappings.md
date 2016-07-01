| S3 Operation                 | Subdomain             | S3 Request               | Manta Operation                        | Manta Request                               | Notes                                                   | 
|------------------------------|-----------------------|--------------------------|----------------------------------------|---------------------------------------------|---------------------------------------------------------| 
| Ping                         | $config.baseSubdomain | HEAD /                   | N/A                                    | N/A                                         | Emulate s3 behavior of returning HTTP 405               | 
| Ping                         | $bucket               | HEAD /                   | Verifies $config.bucketPath exists     | HEAD /$config.bucketPath                    | Emulate s3 behavior of returning HTTP 200 if exists     | 
| List all buckets             | $config.baseSubdomain | GET /                    | List directories                       | GET /$config.bucketPath                     | S3 Creation Date is mapped to Manta Modification Time   | 
| List bucket contents         | $config.baseSubdomain | GET /$bucket             | Find on contents of $config.bucketPath | GET /$config.bucketPath/$bucket (recursive) | Mfind on contents of bucket directory                   | 
| List bucket contents         | $bucket               | GET /                    | Find on contents of $config.bucketPath | GET /$config.bucketPath/$bucket (recursive) | Mfind on contents of bucket directory                   | 
| Get a directory as an object | $config.baseSubdomain | GET /$bucket/$objPath    | Get directory                          | GET /$config.bucketPath/$bucket/$objPath    | Emulate s3 behavior of returning HTTP 404               | 
| Get a directory as an object | $bucket               | GET /$objPath            | Get directory                          | GET /$config.bucketPath/$bucket/$objPath    | Emulate s3 behavior of returning HTTP 404               | 
| Get an object                | $config.baseSubdomain | GET /$bucket/$objPath    | Get object                             | GET /$config.bucketPath/$bucket/$objPath    | Returns object                                          | 
| Get an object                | $bucket               | GET /$objPath            | Get object                             | GET /$config.bucketPath/$bucket/$objPath    | Returns object                                          | 
| Create a bucket              | $config.baseSubdomain | PUT /$bucket             | Create directory                       | PUT /$config.bucketPath/$bucket             | Bucket is just a subdirectory of the $config.bucketPath | 
| Create a bucket              | $bucket               | PUT /                    | Create directory                       | PUT /$config.bucketPath/$bucket             | Bucket is just a subdirectory of the $config.bucketPath | 
| Add/overwrite an object      | $config.baseSubdomain | PUT /$bucket/$objPath    | Add/overwrite object                   | PUT /$config.bucketPath/$bucket/$objPath    | mkdirp all dependent directories                        | 
| Add/overwrite an object      | $bucket               | PUT /$objPath            | Add/overwrite object                   | PUT /$config.bucketPath/$bucket/$objPath    | mkdirp all dependent directories                        | 
| Delete a bucket              | $config.baseSubdomain | DELETE /$bucket          | Delete directory                       | DELETE /$config.bucketPath/$bucket          | Deletes the $bucket subdirectory from $bucketPath       | 
| Delete a bucket              | $bucket               | DELETE /                 | Delete directory                       | DELETE /$config.bucketPath/$bucket          | Deletes the $bucket subdirectory from $bucketPath       | 
| Delete a object              | $config.baseSubdomain | DELETE /$bucket/$objPath | Delete object                          | DELETE /$config.bucketPath/$bucket/$objPath | Deletes the object from the $bucket directory           | 
| Delete a object              | $bucket               | DELETE /$objPath         | Delete object                          | DELETE /$config.bucketPath/$bucket/$objPath | Deletes the object from the $bucket directory           | 