# AWS IAM Implementation for Zappy e-Bank

This guide provides a step-by-step walkthrough for setting up AWS Identity and Access Management (IAM) for a fintech startup, Zappy e-Bank. The implementation focuses on creating secure access for backend developers and data analysts while following AWS best practices.

---

## Prerequisites

- An active AWS account with administrator privileges
- Basic understanding of AWS services (EC2, S3, MFA)
- AWS Management Console access

---

## IAM User Creation

### Step 1: Access IAM Dashboard
1. Log in to your 
![AWS Management Console](./img/1.%20AWS%20Console%20login.jpg)
![AWS Management Console](./img/2.%20Sign%20as%20root.jpg)
![AWS Management Console](./img/3.%20Sign%20in.jpg)


---

## Step 2: Access AWS Console & IAM Dashboard
*Best Practice: Always start from the official AWS Console to ensure you're in the correct environment*

1. **Log in to AWS Console**  
   ![AWS Management Console](./img/4.%20AWS%20Console%20welcome%20page.jpg) 
   *Navigate to Services > Security > IAM*

2. **Access IAM Dashboard**  
   ![IAM Dashboard](./img/5.%20I%20AM%20Dashboard.jpg)  
   *Verify you're in the correct region (top-right corner)*

---

## 🔑 Step 3: Create Custom Policies
*Best Practice: Create least-privilege policies instead of using broad managed policies*

### For Developers (EC2 Access):
1. Navigate to **Policies > Create policy**  
   ![Policy Section](./img/6.%20Policy.jpg)

2. Select **Create policy**  
   ![Create Policy Button](./img/7.%20create%20policy.jpg)

3. Choose service: **EC2**  
   ![Select EC2 Service](./img/8.%20Choose%20a%20service.jpg)

4. Select actions: **All EC2 actions**  
   ![Select EC2 Actions](./img/9.%20all%20ec2%20action.jpg)

5. Set resources: **All resources**  
   *Production Tip: Restrict to specific resource ARNs later*  
   ![Resource Selection](./img/10a%20All%20resource.jpg)

6. Review and name: `developers-policy`  
   ![Policy Review](./img/11.%20Create%20policy.jpg)

7. Verify policy creation  
   ![Policy Verification](./img/12.%20verifying%20policy.jpg)

### For Analysts (S3 Access):
Repeat steps 1-7 using:
- Service: **S3**
- Name: `analyst-policy`

1. Navigate to **Policies > Create policy**  
   ![Policy Section](./img/13.%20policy%20for%20analyst.jpg)

2. Select **Create policy**  
   ![Create Policy Button](./img/14.%20creating%20the%20policy.jpg)

---

## 👥 Step 4: Create Groups & Attach Policies
*Best Practice: Group-based permissions enable scalable access management*

1. Go to **User Groups > Create group**

2. For developers:
   - Name: `Development-Team`
   - Attach policy: `developers-policy`
   ![Create User Groups](./img/15.%20Create%20Group.jpg)
   ![Create User Groups](./img/16.%20Policy%20attached%20to%20Developer-Team.jpg)
   ![Create User Groups](./img/16.%20Policy%20attached%20to%20Developer-Team.jpg)

3. For analysts:
   - Name: `Analyst-Team`
   - Attach policy: `analyst-policy`
   ![Create User Groupsn](./img/17.%20Assigning%20Policy%20to%20Analyst%20Group.jpg)

---

## 👤 Step 5: Create Users & Assign Groups
*Best Practice: Always enable console access and auto-generated passwords*

### For John (Developer):
1. **Users > Create user**
2. Name: `John`
3. Enable **AWS Management Console access**
4. Select **Custom password** and set strong password
5. Add to group: `Development-Team`
6. **Download .csv credentials** (store securely)
 ![Create User Groupsn](./img/18.%20Creating%20Users.jpg)
  ![Create User Groupsn](./img/19.%20Creating%20user.jpg)
   ![Create User Groupsn](./img/20.%20Setting%20Permission.jpg)
    ![Create User Groupsn](./img/21.%20Review%20and%20Create.jpg)

### For Mary (Analyst):
Repeat with:
- Name: `Mary`
- Group: `Analyst-Team`
![Create User Groupsn](./img/22.%20Creating%20User%20Mary.jpg)
![Create User Groupsn](./img/23.%20Setting%20permission%20for%20user%20mary.jpg)
![Create User Groupsn](./img/24.%20Review%20and%20create.jpg)
![Create User Groupsn](./img/25.%20User%20mary%20created.jpg)

---

## Step 6: Test Access
*Best Practice: Always verify permissions before production use*

### Test John's Access:
1. Login as John using console link
![Test Access](./img/26.%20testing%20user%20john.jpg)
![Test Access](./img/27.%20user%20login%20page.jpg)
![Test Access](./img/28.%20Password%20reset%20for%20john.jpg)
![Test Access](./img/29.%20John%20Consloe%20page.jpg)
![Test Access](./img/30.%20John%20Lunch%20EC2.jpg)
![Test Access](./img/31.%20creating%20key%20pair.jpg)
![Test Access](./img/32.%20john%20creating%20EC2%20ready%20to%20lunch.jpg)
2. Verify:
   - ✅ Can access EC2 dashboard and Lunch EC2 instance
   - ❌ Cannot access S3 dashboard
   ![Test Access](./img/33.%20User%20John%20Created%20EC2.jpg)     
   ![Test Access](./img/34.%20John%20Deleted%20the%20instance%20to%20prevent%20any%20billing%20from%20AWS.jpg)
   

### Test Mary's Access:
1. Login as Mary
![Test Access Mary](./img/35.%20Testing%20user%20Mary%20login.jpg)
![Test Access Mary](./img/36.%20Mary%20loging%20page.jpg)
![Test Access Mary](./img/37.%20changing%20password.jpg)
![Test Access Mary](./img/38.%20Password%20change%20done.jpg)
![Test Access Mary](./img/39.%20mary%20Console.jpg)
![Test Access Mary](./img/40.%20Ceating%20an%20S3%20bucket.jpg)
![Test Access Mary](./img/41.%20Preview%20s3%20bucket.jpg)

2. Verify:
   - ✅ Can access S3 dashboard
   ![Test Access Mary](./img/41.%20Preview%20s3%20bucket.jpg)
   ![Test Access Mary](./img/42.%20Deleting%20s3%20Bucket.jpg)
   ![Test Access Mary](./img/43.%20S3%20Bucket%20successfully%20created.jpg)
   ![Test Access Mary](./img/44.%20S3%20Bucket%20successfully%20deleted.jpg)
   
   - ❌ Cannot launch EC2 instances

---

## 🔒 Step 7: Enable MFA Security
*Critical Practice: MFA adds essential protection for financial data*

1. Go to **John's user > Security credentials**
2. Select **Manage MFA**
3. Choose **Virtual MFA device**
4. Scan QR code with authenticator app (Google/Microsoft Authenticator)
5. Enter two consecutive codes
6. Repeat for all human users
![Test Access Mary](./img/45.%20Enablling%20MFA%20for%20john.jpg)
   ![Test Access Mary](./img/46.%20MFA%20for%20john.jpg)
   ![Test Access Mary](./img/47.%20Auth%20app.jpg)
   ![Test Access Mary](./img/48.%20MFA%20Assigned%20for%20john.jpg)
   ![Test Access Mary](./img/49.%20Creating%20MFA%20for%20mary.jpg)
   ![Test Access Mary](./img/50.%20Selecting%20MFA%20App.jpg)
   ![Test Access Mary](./img/52.%20MFA%20successful.jpg)
   
---

## 📈 Scaling Your Team
To add new members:
1. Create user
2. Add to appropriate group
3. Enable MFA
4. Provide credentials via secure channel

### Project Reflection: AWS IAM Implementation for Zappy e-Bank

---

#### **1. Role of IAM in AWS**  
IAM (Identity and Access Management) is AWS's core security service that:  
- **Controls access** to AWS resources through authentication (sign-in) and authorization (permissions).  
- **Centralizes management** of users, groups, roles, and policies.  
- **Enables least privilege** by granting only necessary permissions.  
- **Supports compliance** (e.g., PCI-DSS for fintech) via audit trails and granular controls.  

**Security Contribution**:  
- Prevents unauthorized access to sensitive data (e.g., financial records).  
- Secures root accounts with MFA and policy-based access restrictions.  
- Allows resource isolation through service-specific permissions.  

---

#### **2. IAM Users vs. Groups**  
| **IAM Users** | **IAM Groups** |  
|---------------|---------------|  
| Unique identities for *individuals* (e.g., `John`, `Mary`). | Collections of users *sharing permissions* (e.g., `Development-Team`). |  
| Have long-term credentials (password/access keys). | *No credentials* – only permission containers. |  
| Best for *service accounts* (e.g., CI/CD bots). | Best for *human roles* (e.g., developers, analysts). |  

**When to Use**:  
- **Create a User**: For individual service accounts or contractors needing unique access.  
- **Use Groups**: For teams requiring identical permissions (e.g., 10 developers needing EC2 access).  

---

#### **3. Creating IAM Policies**  
**Process for Custom Policies**:  
1. **Define Requirements**: Identify needed permissions (e.g., "EC2 full access for developers").  
2. **Create Policy**:  
   - Navigate to **IAM > Policies > Create policy**.  
   - Select service (e.g., EC2).  
   - Choose actions (e.g., `ec2:*` for full access).  
   - Specify resources (e.g., "All resources" for sandbox; restrict to ARNs in production).  
3. **Attach Policy**:  
   - Link to groups (e.g., attach `developers-policy` to `Development-Team`).  
   - *Avoid attaching directly to users* for scalability.  

**Key Considerations**:  
- Start with AWS-managed policies (e.g., `AmazonS3FullAccess`) and customize if needed.  
- Use IAM Access Analyzer to validate permissions.  

---

#### **4. Principle of Least Privilege**  
**Definition**: Grant users *only the permissions essential* for their role.  

**Significance in AWS**:  
- **Reduces attack surface**: Limits damage from compromised credentials (e.g., a developer can't delete S3 buckets).  
- **Enforces compliance**: Critical for fintech (e.g., prevents unauthorized access to customer data).  
- **Simplifies auditing**: Clear permission boundaries ease security reviews.  

**Implementation**:  
- Restrict policies (e.g., `analyst-policy` allows S3 access but blocks EC2).  
- Use permission boundaries for admin roles.  

---

#### **5. Reflection: John & Mary Scenario**  
**Configurations**:  
| **Role** | **IAM Setup** | **Alignment with Job Functions** |  
|----------|---------------|----------------------------------|  
| **John** (Developer) | - User: `John`<br>- Group: `Development-Team`<br>- Policy: `developers-policy` (EC2 access) | Can launch/manage EC2 instances for application deployment but *cannot* access financial data in S3. |  
| **Mary** (Analyst) | - User: `Mary`<br>- Group: `Analyst-Team`<br>- Policy: `analyst-policy` (S3 access) | Can read/write to S3 buckets for data analysis but *cannot* modify infrastructure. |  

**Principle of Least Privilege**:  
- **John**: EC2 access supports backend development without exposing financial data stores.  
- **Mary**: S3 access enables data analysis without risking infrastructure changes.  
- **Validation**: Testing confirmed John could launch EC2 but not access S3, while Mary could access S3 but not EC2.  

**Security Impact**:  
- Isolates sensitive financial data from developers.  
- Prevents accidental/malicious cross-service access.  

> **Key Insight**: Group-based policies ensure scalable security – adding 10 new developers only requires adding them to `Development-Team`, avoiding individual policy management.

Final Assessment:

950
👍🏽 Excellent
Review
The provided implementation of the AWS IAM setup for Zappy e-Bank effectively adheres to the instructor's instructions and project requirements, covering the creation of users, policies, groups, and proper testing to ensure security compliance. The write-up demonstrates a thorough understanding of IAM features and principles, following best practices like enforcing the least privilege, enabling MFA, and using group-based permissions to simplify user management.

Strengths:
Purpose Alignment: The submission fully addresses the instructor's requirement to create an IAM setup for John and Mary in a well-delineated manner, ensuring their access aligns with their roles as a Developer and Data Analyst, respectively.
Detailed Execution: The setup instructions are comprehensive and accompanied by clear screenshots for better understanding. For example, creating custom policies for EC2 and S3 and attaching those to the appropriate groups were well explained and implemented.
Testing and Validation: Testing steps for verifying role-specific access were clearly outlined and demonstrated, ensuring John can only access EC2 resources and Mary can only access S3 resources. The screenshots support the validation.
Reflection and Security Principles: The project reflection provides a strong theoretical backing for the practical steps, emphasizing IAM’s role in security, the least privilege concept, and the differentiation between users and groups.
MFA Enforcement: MFA configurations for both users add an essential layer of security, particularly important for a fintech organization handling sensitive financial data.
Weaknesses:
Policy Restrictions: While resource-specific restrictions were suggested, the policies use "All resources," which goes against the principle of least privilege. Narrowing permissions to specific ARNs would enhance security, especially in a production environment.
Advanced MFA Configurations: Although the submission includes MFA, it could further expand by introducing conditional access policies, such as restricting login from untrusted IPs or regions.
Component Scores (Out of 100 for each criterion):
Purpose (25/25): The project clearly meets the purpose of creating an IAM framework adhering to job roles.
Requirements (23/25): The required functionalities, such as user and group setup, policy attachment, and MFA enablement, were completed. However, overly broad resource definitions leave room for improvement.
Use Case (24/25): The setup appropriately matches the use case of a secure, role-specific access policy for a fintech application. A slight deduction is for skipping finer security tuning.
Performance (23/25): While the platform functionality was validated successfully, introducing resource-level constraints and conditional MFA would enhance the overall robustness.
Final Assessment:
The submission is well-rounded and fulfills the criteria with minor areas to improve in terms of policy refinement and advanced security configurations. The attached images significantly assist in validating the steps and ensuring the solution works as intended.


Feed Back
No feedback returned