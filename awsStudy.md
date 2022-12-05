- AZ - > Availability Zones


# IAM: User and groups
- IAM -> Identity and Access Management, global service
- Root account -> crated by default, shouldn't userd or shared
- Users/groups can be assinged to json documentos(policy), to refeer whith user/group is allowed to do on the AWS account
- Policies define permissions, don't give more permission that the user needs
- IAM is a global service, users and groups are created in a global fashion

https://edvargas-aws.signin.aws.amazon.com/console

- IAM policies structure:
    - Version - policiy language version, awways inclde "2012-10-17"
    - Id - identifier optional
    - Statment - one or more (required)
        - Sid: an identifier for the statment (optional)
        - Effect: whether the statement allows or denies access(Allow,Deny)
        - Principal: account/user/role to whitch this policy applied to
        - Action: list of actions
        - Resource: list of resources
        - Condition: condition for when this policy is in effect

- Password policy -> in AWS we can set a password policy, increasing the security for the account. We can setupt a password policy
    - Minimum password length
    - Require specific character types
    - Allow all IAM user to change their own passwords
    - Password expiration
    - Prevent password re-use

- Multi Factor Authentication - MFA
    - combination password + secutiry device you own
    - Virtual MFA device: an APP in the smartphone like Google authenticatior
    - Universal 2nd Factor(U2F)/Hardware Key fob MFA: it's an phisycal key make by a 3rd party company, support for multiple root and IAM users using a single key

- IAM Roles for Services
    - Some AWS services will need to perform some actions on your behald, for Eg.: EC2 instance escalating memory
    - To do so, we will assing permissions to AWS services with IAM Roles
    - Commons roles:
        - EC2 Instance roles
        - Lambda Function roles
        - Roles for CloudFormation


# AWS CLI - Command line interface and AWS Shell
- AWS CLI/Shel are tools that enables to interact with AWS services using commands in your command-line shell
- On AWS CLI case, we need to sing in on terminal, using the user's access key and secret key
- On the AWS Shell case, it's available on the AWS console for specific regions

