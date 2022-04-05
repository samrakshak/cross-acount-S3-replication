
# Cross Account S3 Replication 


First step is to do a aws cli command i.e.: aws s3 sync
command to copy the content form source to destination

Second step will be to enable replication. Go through the following steps

Important information before starting:
 
 - Make sure both the s3 bucket (source and destination) have versioning enabled.If not
   enabled then select the bucket > properties > scroll to get the Bucket versioning section >
   edit > enable versioning
 
 - aws cli is installed and configured
 - aws credentials contain profiles for both “source” and “destination” accounts
 - profiles have necessary permissions for buckets access, IAM role creation, put bucket policy, create replication rule etc


 Clone the repository and run the following command

 ```sh
 ❯ ls
createS3Replication.sh  README.md  templates
❯ ./createS3Replication.sh source_account_profile destination_account_profile env source_bucket_name destination_bucket_name
 ```

 - source_account_profile aws credentials profile for the source account
 - destination_account_profile aws credentials profile for the destination account
 - env dev/test/uat etc. Used in the role name
 - source_bucket_name name of the source bucket in the source account
 - destination_bucket_name name of destination-bucket in the destination account

 Example:
   - First set both the aws profile 

   ```sh
    > cat ~/.aws/credentials
     
     [main-account]
      aws_access_key_id = <Access-Key>
      aws_secret_access_key = <Secret-Key>

     [replica-account]
      aws_access_key_id = <Access-Key>
      aws_secret_access_key = <Secret-Key>


   ```

 ```sh
   ./createS3Replication.sh main-account replica-account dev important-reports-bucket replica-for-important-reports-bucker
 ```


 # Result

  After running the script select the master account bucket then 
   then > management > Replication Rules > and check the replication rule that has been created 


Now we can test the replication create objects in master s3 bucket 
and after some time it gets replicated to the target s3 bucket too.
Same way deleting objects also works ....
It may take upto 5 to 15 mins for replication depending on the size of the object
