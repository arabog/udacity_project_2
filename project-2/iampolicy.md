To create an IAM role and instance profile (AWS CLI)

1. Create the following trust policy and save it in a text file named ec2-role-trust-policy.json.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com"},
      "Action": "sts:AssumeRole"
    }
  ]
}


2. Create the s3access role and specify the trust policy that you created using the create-role command.

aws iam create-role  --role-name s3access  --assume-role-policy-document file://ec2-role-trust-policy.json


3. Create an access policy and save it in a text file named ec2-role-access-policy.json. For example, this policy grants administrative permissions for Amazon S3 to applications running on the instance.

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:*"],
      "Resource": ["*"]
    }
  ]
}

4. Attach the access policy to the role using the put-role-policy command.

aws iam put-role-policy --role-name s3access --policy-name S3-Permissions --policy-document file://ec2-role-access-policy.json


5. Create an instance profile named s3access-profile using the create-instance-profile command.

aws iam create-instance-profile --instance-profile-name s3access-profile

6. Add the s3access role to the s3access-profile instance profile.

aws iam add-role-to-instance-profile --instance-profile-name s3access-profile --role-name s3access

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html