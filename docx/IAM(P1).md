# Khái niệm
- AWS Identiry and Access Management (IAM) là một web service giúp bạn kiểm soát truy cập vào các dịch vụ trên AWS. Nó được sử dụng để điều khiển việc ai có thể truy cập và có quyền gì tới các resource.
- Tính năng:
  - ```Share access to your AWS account```: Cấp quyên sử dụng tài nguyên trong aws account của bạn mà không cần chia sẻ password or access key.
  - ```Granular permissions```: Cấp quyền khác nhau cho các user khác nhau với các resource khác nhau. For example, allow user access to EC2, S3, DynamoDB and another user read-only access just some S3 bucket,..
  - ```Secure access to AWS resources for applications that run on Amazon EC2```: You can use IAM features to securely provide credentials for applications that run on EC2 instances. These credentials provide permissions for your application to access other AWS resources
  - ```Multi-factor authentication (MFA)```: sử dụng phần mềm thứ 3 để tăng độ bảo mật 2 lớp.
  - ```Identitr federation```: You can allow users who already have passwords elsewhere—for example, in your corporate network or with an internet identity provider—to get temporary access to your AWS account.
  - ```Identity information for assurance```: If you use AWS CloudTrail, you receive log records that include information about those who made requests for resources in your account. That information is based on IAM identities.
  - ```PCI DSS Compliance```:
  - ```Integrated with many AWS services```:
  - ```Eventually Consistent```:
  - ```Free to use```:
- Accessing IAM:
  - AWS management console:
  - AWS Command Line Tools:
  - AWS SDKs:
  - IAM HTTPS API:

**How IAM works**
  ![alts](../images/iam_work.png)
  - *TERM*:
    - ```IAM Resources```: The user, group, role, policy, and identity provider objects that are stored in IAM. As with other AWS services, you can add, edit, and remove resources from IAM.

    - ```IAM Identities```: The IAM resource objects that are used to identify and group. You can attach a policy to an IAM identity. These include users, groups, and roles.

    - ```IAM Entities```: The IAM resource objects that AWS uses for authentication. These include IAM users and roles.

    - ```Principals```: A person or application that uses the AWS account root user, an IAM user, or an IAM role to sign in and make requests to AWS. Principals include federated users and assumed roles.
  - *Principal*
    - Best practice: không sử dụng root account cho các task hàng ngày, thay vào đó tạo IAM identity đẻ sử dụng.
  - *Request*
    - Khi 1 ```pricipal``` sử dụng AWS Management console, AWS API, or AWS CLI, ```principal``` gửi 1 request tới AWS. Request bao gồm các thong tin:
      - ```Acction and Operations```: các hành động hay hoạt động mà ```principal``` muốn thực hiện.
      - ```Resources```: AWS resource dựa  trên các hoạt động hay hành động được thực hiện.
      - ```Principal```: People or application sử dụng user or role để gửi request. Thông tin của principal bao gồm policies liên kết với các thực thể (user or role) mà pricipal đã sử dụng để sing in
      - ```Environment data```: Thông tin về IP address, user agent, SSL enable status or the time of day
      - ```Resource data```: Dữ liệu liên quan đến tài nguyên được yêu cầu.
  - *Authentication*: 
    - Một principal phải được authen sử dụng thông tin xác thực của nó để send request tới AWS.
  - *Authorization*:
    - You must also be authorized (allowed) to complete your request. During authorization, AWS uses values from the request context to check for policies that apply to the request. It then uses the policies to determine whether to allow or deny the request. Most policies are stored in AWS as JSON documents and specify the permissions for principal entities. [Learn more](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html#intro-structure-principal).
  - *Actions or operations*
    - Sau khi request đã được authenticated and authorized, AWS sẽ chập nhận các hoạt động hoặc hành động trên request. Operation định nghĩa những thứ mà bạn có thể làm với resource như viewing, creating, editting, and deleting resources.
  - *Resources*:
    - Sau khi chấp nhận operations trong request, bạn có thể thực hiện các resource liên quan.
# IAM user & group:
- IAM user là một thực thể mà bạn tạo trên AWS để đại diện cho một người hoặc một ứng dụng mà sử dụng để tương tác với AWS.
- Một user trên AWS bao gồm tên và credentials.
- Một User với quyền administratos thì không giống với AWS root account. Detail, see: [AWS root account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)
- Groups là một nhóm các user, cho phép chỉ định quyền cho multi user. Điều này giúp quản llý quyền cho những user đó dễ dàng hơn
- Groups chỉ chứa user, không thể chứa groups khác
- Một user có thể không thuộc group nào và cũng có thể thuộc nhiều groups khác nhau
  ![alts](../images/user-groups.png)
*Tham khảo thêm về [user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) và [group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)*
# IAM permission & policy
- Bạn quản lý truy cập trên AWS bằng việc tạo policy và gán chúng tới IAM identites(users, groups of user, role) hoặc các AWS resources.
- Một policy là một object trên AWS, khi kết hợp với identity or resource sẽ define ra permission của nó. 
- Policy được lưu trữ dưới dạng JSON document. Hỗ trợ 6 loại policy:
  - ```Identity-based policies```: Gắn managed or inline policy tới IAM identity (user, groups of user, roles).
    - ```Managed policies```: standalone identity-based policies mà bạn có thể attach tới multiple users, groups, và roles trên tài khoản AWS. Có 2 loại managed policies:
      - ```AWS Managed policies```: Created and managed AWS
      - ```Customer managed policies```: bạn tạo và quản lý trong tài khản aws. Bạn có thể cung cấp các policies với quyền tối thiểu khi sử dụng loại này.
    - ```inline policies```: Policies mà ban add trực tiếp cho user, groups, roles và sẽ bị xóa khi xóa identity.
  - ```Resource-based policies```: Attach inline policies to resources. Phố biến nhất của loại policy này là Amazon S3 bucket policies và IAM role trust policies. Loại policies này gán quyền cho các pricipal cụ thể để thực hiện các action được chỉ định trên resource. Resource-based policies are inline policies. There are no managed resource-based policies. [Learn more](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based)
  - ```IAM permissions boundaries```: bạn đặt quyền tối đa mà chính sách dựa trên danh tính có thể cấp cho một thực thể IAM. Khi bạn set  permissions boundary cho một thực thể, thì thực thể đó chỉ có thể thực hiện các hành động được cho phép bởi cả identity-based policies và permissions boundaries của nó. [Learn more](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_bound)
  - ```Service control policies (SCPs)```: [Tham khảo](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_scp)
  - ```Access control lists (ACLs)```: [Tham khảo](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_acl)
  - ```Session policies```: [Tham khảo](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session)
  
**<ins>Lưu ý</ins>**
  - AWS account root có thể bị ảnh hưởng bời một số loại policies còn loại khác thì không. Ví dụ: không thể attach Identity-based policies tới root user, tuy nhiên có thể chỉ định root user như một principal trong Resource-based policies. A root user is still the member of an account. If that account is a member of an organization in AWS Organizations, the root user is affected by any SCPs for the account.

# JSON policy document structure
- Một JSON policy document bao gồm:
  ![alts](../images/json_policy.png)

  - Mỗi một statement bao gồm một permission duy nhất. Nếu một policy bao gồm nhiều statements, AWS sẽ áp dụng toán tử ```OR``` cho các statement.

*Ví dụ*
```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"ec2:Describe*",
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":"elasticloadbalancing:Describe*",
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "cloudwatch:ListMetrics",
            "cloudwatch:GetMetricStatistics",
            "cloudwatch:Describe*"
         ],
         "Resource":"*"
      }
   ]
}
```

 - ```Version```: Định nghĩa version của policy language mà bạn muốn sử dụng. As a best practice, use the latest 2012-10-17 version.
 - ```Statement```: Main policy element để chứa các element sau. Có thể bao gồm 1 or nhiều statement trong 1 policy
 - ```Sid```(Optional): phân biệt giữa các statement
 - ```Effect```: Sử dụng ```Allow``` or ```Deny``` để chỉ ra hành động cho phép hay từ chối
 - ```Principal``` (Required in only some circumstances): Nếu sử dụng resource-based policy, cần phải chỉ định account, user, role or federated user mà bạn muốn cho phép hay từ chối. IAM policy không thể chứ element này.
 - ```Action```: Danh sách các action mà policy allow hay deny.
 - ```Resource``` (Required in only some circumstances): Với IAM polcy, càn chỉ định một danh sách các resource cụ thể đẻ áp dụng cho action. Với resource-based policy, element này là optional. Nếu không có thì resource mà acction apply là resource mà policy attach.
 - ```Condition```: Specify the circumstances under which the policy grants permission.

# Multiple statements and multiple policies

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FirstStatement",
      "Effect": "Allow",
      "Action": ["iam:ChangePassword"],
      "Resource": "*"
    },
    {
      "Sid": "SecondStatement",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ThirdStatement",
      "Effect": "Allow",
      "Action": [
        "s3:List*",
        "s3:Get*"
      ],
      "Resource": [
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ],
      "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}
    }
  ]
}
```

# Tham khảo:
- https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html
- https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html#intro-structure-principal