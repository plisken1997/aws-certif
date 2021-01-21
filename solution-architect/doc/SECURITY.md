SECURITY
=======

## IAM Policies 
* resource identified by ARN: arn:partition:service:region:account_id:resource => arn:{aws|aws-cn}:ec2:eu-west-3:901806327172:{resource}
* region could be empty: arn:partition:service::account_id:resource
* s3 specific: no region neither account id: arn:aws:s3:::my-bucket
* policy: a JSON document where reside persmissions
    * identity policy
    * resource policy (eg. s3 bucket)
    * inline policy: policy just attached to a single role
* permission boundaries: delegate administration to the users; used to limit permissions (???)


## AWS Directory Service
* connect AWS resources to on-premises AD
* AD is based on LDAP & DNS
* AD connector (proxy to  on prem) to join AWS AD & on prem AD
* cloud directoy: directory based store for developers
* AD comptatible: managed MS AD, AD connector, Simple AD, Amazon quicksight or workspaces
* AD NOT comptatible: cognito users pools, cloud directory
* AD Trust: extends existing AD to on prem AD (???)


## AWS Resource Access Manager
* grant access to another aws account to our resources (eg. aurora DB)
* account 2 must accept an invitation from the shared account to see the shared resources


## SSO
* centrally manage access to applications
* use existing corporate entities
* SAML
* all sign on are recorded by cloudtrail


## Web Identity Federation
* access to aws resources after the user had successfully login to another provider (eg. fb, gg, etc.)


## cognito
* web identity federation service
* sign-up and sign-in to your app
* act as an identity broker between app and web id providers
* cognito user pool: handle registration, authentication & account recovery
    * cognito user act as an identity provider between identity provider and AWS => produce a JWT
* cognito identity pool: grant authorisation to aws resources => create temporary aws identity pool

---
[Back](/solution-architect)
