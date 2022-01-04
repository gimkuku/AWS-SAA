[##_Image|kage@bSqldO/btrpMPTWbzA/71fuso2dgnDPvu4GJeKddK/img.png|CDM|1.3|{"originWidth":400,"originHeight":373,"style":"alignCenter"}_##]

#### IAM

\- Identity and Access Management 

\- Global service : 사용 시 region을 설정하지 않아도 됨

\- Root account 는 회원 가입 시 생성, 공유하면 안됨

\- Root account를 사용하지 않고, 매 physical user마다 user를 생성하여 관리하는 것이 좋음

#### User & Group

\- User : 구성의 사람들. 

\- Group : 사람들을 묶어 권한 관리를 쉽게 할 수 있음

\- Group은 오직 사람만 담을 수 있고 다른 Group은 Group에 속할 수 없음

\- User는 여러 Group에 속할수도, Group에 속하지 않을 수도 있음

#### Policy

\- JSON documents that define a set of permissions for making requests to AWS services and can be used by IAM Users, User Groups and IAM roles.

\- AWS 서비스를 사용하기 위한 권한을 적은 JSON문서로, IAM User, Group, Role에서 사용됨

\- 항상 least privilege principle, 즉 최소한의 권한만 주는 것이 올바름

\- Inline Policy : Policy가 user에게 직접 속함

**Policy의 구조**

```
{
    "Version" : "2012-10-17",
    "Id" : "S3-account-permissions",
    "Statement" :[
        {      
            "Sid": "1",
            "Effect" : "Allow,
            "Principal":{
                 "AWS": ["arn:aws:iam::123123123:root"]
            },
            "Action" : "ec2:Describe*",
            "Resource" : ["arn:aws:s3:::image/*"],
            "Condition" : { "StringEquals" : { "aws:username" : "johndoe" }}
        },
        {      
            "Effect" : "Allow,
            "Action" : "ec2:Describe*",
            "Resource" : "*"
        },
    ]
}
```

Policy 

   - Version : policy language version, always include "2012-10-17"

   - Id : identifier for the policy

   - Statement. : one or more individual statements

        - Sid : an identifier for the statement

        - Effect : Allow / Deny

        - Principal : account/user/role to which this policy applied to

        - Action : list of actions this policy allows or denies

        - Resource : list of resources to which the action applied to

        - Condition :  conditions for when this policy is in effect - when/ which

#### Access Key

\- AWS Management Console : protected by password + MFA

\- AWS command Line Interface(CLI) : protected by **access keys**

       \- 터미널에서 사용할 수 있음

\- AWS Software Developer Kit (SDK) : for code: protected by **access keys** 

       \- 개발을 편리하게 하기 위해 만들어 둔 API들 ( 라이브러리의 집합)

**access keys**

\- generated through the AWS Console

\- 공유 x

\- Access Key ID : username 같은거

\- Secret Access Key : password 같은거

####   
Role

\- user 랑 비슷한데, 사람이 사용하는 게 아니라 AWS service에 붙여 권한을 줄 수 있음

\- EC2 Instance Roles

\- Lambda Function Roles

\- Roles for. CloudFormation 

#### Security Tool

**IAM Credentials Report**

\- 계정 단위

\- 계정의 유저가 쓰는지, 비번을 바꾸었는지, credential의 상태 (만료됐는지) 등을 확인할 수 있음

**IAM Access Advisor**

\- User 단위

\- 어떤 policy를 사용하여 어떤 기능을 사용하는지, 어떤 기능을 안쓰는지 등을 확인할 수 있음

\- 최소한의 권한만을 policy에 적기 위해 참고함
