# Header
<p align="center">
  
</p>

# Okta Admin Roles and Delegation

This lab delegates admin access in Okta for the fictional company Kennon Technologies. It covers standard admin roles, scoping a role to a single group, building a custom role with a resource set, limiting an app administrator to one app, and assigning roles through a group. Each assignment is tested by signing in as the test admin and confirming what they can and can't do.

---

## Skills Demonstrated

- Assigning standard admin roles such as Read Only, Help Desk and Application Administrator
- Scoping admin roles to specific groups and apps
- Building custom admin roles with only the permissions needed
- Creating resource sets to limit which users a custom role can manage
- Assigning admin roles through group membership
- Applying least privilege to admin access
- Verifying role grants, revocations and delegated actions in the System Log

---

## Prerequisites

- An Okta Integrator Free Plan org (Identity Engine) with Super Administrator access
- Okta Verify installed on a phone
- An email inbox you control for the test users' primary emails
- A second browser session signed in as your admin account while assigning roles

---

## Environment

| Component | Details |
|---|---|
| Identity Platform | Okta Integrator Free Plan (Identity Engine) |
| Company | Kennon Technologies (kennon.tech) |
| Test Admins | Riley Carter, Sam Patel, Taylor Reed |
| Managed Users | Avery Brooks (Finance), Casey Nguyen (Sales) |
| Groups | Kennon Finance, Kennon Sales, Kennon Help Desk Admins |
| Test Apps | Kennon Payroll, Kennon CRM (Bookmark Apps) |
| Custom Admin Objects | Kennon Password Reset (role), Kennon Sales Resources (resource set) |
| MFA | Okta Verify |

> **Note:** Real IP addresses and email addresses are blurred in screenshots. Bookmark apps appear as **Bookmark App** in the System Log.

---

## Table of Contents

1. [Prepare Users and Groups](#1-prepare-users-and-groups)
2. [Standard Admin Roles](#2-standard-admin-roles)
3. [Scoped Delegation with a Standard Role](#3-scoped-delegation-with-a-standard-role)
4. [Custom Admin Role and Resource Set](#4-custom-admin-role-and-resource-set)
5. [App Administrator Scope to One App](#5-app-administrator-scope-to-one-app)
6. [Assign Roles Through a Group](#6-assign-roles-through-a-group)
7. [Test Matrix Summary](#7-test-matrix-summary)
8. [Cleanup](#8-cleanup)
9. [Closing Note](#closing-note)

---

## 1) Prepare Users and Groups

This phase creates the groups and users that delegated admins will manage, along with three test admins who receive roles in later phases. Each test admin enrolls in Okta Verify, since the Okta Admin Console requires MFA.

### Create Target Groups

1. In the **Okta Administrator Console** open the **Directory** dropdown then **Groups**
2. Press **Add Group** then create the first group with the following information then **Save:**

   - **Name:** Kennon Finance
   - **Description:** Finance users managed by delegated admins
  
# Image 1

3. Press **Add Group** again then create the second group with the following information then **Save:**

   - **Name:** Kennon Sales
   - **Description:** Sales users managed by delegated admins
  
# Image 2
# Image 3

### Create Test Users

1. Open the **Directory** dropdown then **People** then press **Add Person**
2. Create the first user with the first user with the following information then **Save and Add Another:**

   - **First Name:** Avery
   - **Last Name:** Brooks
   - **Username:** avery.brooks@kennon.tech
   - **Primary Email:** yourname+avery@gmail.com
   - **Groups:** Kennon Finance
   - **Activation:** Activate Now
   - **I Will Set Password:** Check
   - **User Must Change Password on First Login:** Uncheck
  
# Image 4

3. Create the second user with the following information then **Save:**

   - **First Name:** Casey
   - **Last Name:** Nguyen
   - **Username:** casey.nguyen@kennon.tech
   - **Primary Email:** yourname+casey@gmail.com
   - **Groups:** Kennon Sales
   - **Activation:** Activate Now
   - **I Will Set Password:** Check
   - **User Must Change Password on First Login:** Uncheck

# Image 5

### Create Test Admins

1. Create the first admin with the following information then **Save and Add Another:**

   - **First Name:** Riley
   - **Last Name:** Carter
   - **Username:** riley.carter@kennon.tech
   - **Primary Email:** yourname+riley@gmail.com
   - **Groups:** Leave Blank
   - **Activation:** Activate Now
   - **I Will Set Password:** Check
   - **User Must Change Password on First Login:** Uncheck

# Image 6

2. Create the second admin with the following information then **Save and Add Another:**

   - **First Name:** Sam
   - **Last Name:** Patel
   - **Username:** sam.patel@kennon.tech
   - **Primary Email:** yourname+sam@gmail.com
   - **Groups:** Leave Blank
   - **Activation:** Activate Now
   - **I Will Set Password:** Check
   - **User Must Change Password on First Login:** Uncheck

# Image 7

3. Create the third admin with the following information then **Save:**

   - **First Name:** Taylor
   - **Last Name:** Reed
   - **Username:** taylor.reed@kennon.tech
   - **Primary Email:** yourname+taylor@gmail.com
   - **Groups:** Leave Blank
   - **Activation:** Activate Now
   - **I Will Set Password:** Check
   - **User Must Change Password on First Login:** Uncheck

# Image 8
# Image 9

### Enroll Okta Verify for the Test Admins

1. Log in to **Okta** as **Riley Carter** using an **incognito** browser
2. Set up **Okta Verify** then **Sign Out**

# Image 10

3. Log in to **Okta** as **Sam Patel** using an **incognito** browser
4. Set up **Okta Verify** then **Sign Out**

# Image 11

5. Log in to **Okta** as **Taylor Reed** using an **incognito** browser
6. Set up **Okta Verify** then **Sign Out**

# Image 12

### Verify in the System Log

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. Search for user creation events to confirm all five users were created:
   ```text
   eventType eq "user.lifecycle.create"
   ```
   **Create okta user** events appear for **Avery Brooks**, **Casey Nguyen**, **Riley Carter**, **Sam Patel** and **Taylor Reed**, with your admin account as the actor

# Image 13
# Image 14

3. Search for authenticator activations to confirm the test admins enrolled Okta Verify:
   ```text
   eventType eq "user.mfa.factor.activate"
   ```
   Three **Activate factor for user** events appear for each test admin with **Okta Verify** as the target, one for each verification method enrolled with the app

# Image 15
# Image 16

---

## 2) Standard Admin Roles

This phase assigns two standard admin roles and compares what each test admin can see and change. Read Only Administrator can view the org but not change it, while Help Desk Administrator can only handle support tasks like password resets.

### Assign Read Only Administrator to Sam Patel

1. In the **Okta Administrator Console** open the **Security** dropdown then **Administrators** then open the **Admins** tab
2. Press **Add Administrator** under **Select Admin** choose **Sam Patel**
3. Under **Role** choose **Read-Only Administrator** then **Save Changes**


# Image 17

### Assign Help Desk Administrator to Riley Carter

1. Select **Add Administrator** again then under **Select Admin** choose **Riley Carter**
2. Under **Role** choose **Help Desk Administrator** then **Save Changes**

# Image 18

### Test Read Only Administrator as Sam Patel

1. Log in to **Okta** as **Sam Patel** in an **incognito** browser
2. Open the Directory** dropdown then **People** then open **Avery Brooks's** profile
3. Confirm there is no ability to change the account such as; **Reset Password, Deactivate, or Edit Profile**

# Image 19

4. Open the **Security** dropdown then **Authentication Policies**
5. Choose **App Sign-In** then open **Okta Dashboard**
6. Confirm there is no option to edit the policy or add rules to the policy

# Image 20

7. Sign out of Sam Patel's account

### Test Help Desk Administrator as Riley Carter

1. Log in to **Okta** as **Riley Carter** in an **incognito** browser
2. Open the Directory** dropdown then **People** then open **Avery Brooks's** profile
3. Confirm there is an ability to **Reset or Remove Password** and to **Reset Authenticators** but no **Deactivate** account option

# Image 21

4. Select **Reset Password** then **Send a Reset Password Email**

# Image 22

5. Sign out of Riley Carter's account

### Verify in the System Log

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. Search for admin role grants to confirm both standard roles were assigned:
   ```text
   eventType eq "user.account.privilege.grant"
   ```
   **Grant user privilege** events appear for **Sam Patel** and **Riley Carter**, with your admin account as the actor and **Role Assigned** as a target

# Image 23
# Image 24

3. Search for password resets started by Riley Carter to confirm the delegated action:
   ```text
   eventType eq "user.account.reset_password" and actor.alternateId eq "riley.carter@kennon.tech"
   ```
   The password reset event appears with **Riley Carter** as the actor and **Avery Brooks** as the target, showing delegated actions are logged under the admin who performed them

# Image 25
# Image 26

---

## 3) Scoped Delegation with a Standard Role

This phase limits Riley's Help Desk Administrator role from every user to only the Kennon Finance group. Riley keeps the same permissions but can only use them on Finance users.

### Scope Riley Carter to Kennon Finance

1. In the **Okta Administrator Console** open the **Security** dropdown then **Administrators**
2. Open the **Admins** tab then next to **Riley Carter** open the **Edit** dropdown then **Edit Assignment**
3. Next to **Groups** select **Edit**
4. Uncheck the box labeled **Constrain this Role to the Entire Organization**
5. In the **Search by Groups** box search **Kennon Finance** then select **Add** then **Confirm**

# Image 27

### Test Riley on a Finance User

1. Log in to **Riley Carter's** account in an **incognito** browser
2. Open the **Directory** dropdown then select **People** then open **Avery Brooks's** profile
3. Open the **More Actions** dropdown and confirm **Reset Authenticators** appears
4. Press **Resend Password Email** then **Reset Password** then **Sign Out**

# Image 28

### Verify in the System Log

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. Search for privilege changes on Riley Carter to confirm the role was scoped:
   ```text
   eventType sw "user.account.privilege" and target.alternateId eq "riley.carter@kennon.tech"
   ```
   A **Grant user privilege** event appears with **Target assigned to Role** as a target, showing the Help Desk Administrator role was limited to **Kennon Finance**

# Image 29
# Image 30

3. Search for password resets started by Riley Carter to confirm Riley can still act inside the scope:
   ```text
   eventType eq "user.account.reset_password" and actor.alternateId eq "riley.carter@kennon.tech"
   ```
   A new reset event appears for **Avery Brooks** after the scope change, and no reset events appear for **Casey Nguyen**

# Image 31
# Image 32

## 4) Custom Admin Role and Resource Set

This phase builds a custom admin role with only password reset permissions and limits it to Kennon Sales users with a resource set. The custom role defines what an admin can do, and the resource set defines who they can do it to.

### Remove Sam's Read Only Role

1. In the **Okta Administrator Console** open the **Security** dropdown then select **Administrators**
2. Open the **Admins** tab then next to **Sam Patel** open the **Edit** dropdown then select **Edit Assignments**
3. Next to the **Read-Only Administrator** role press the **Trashcan** then **Save Changes**

# Image 33

### Create the Custom Role

1. Open the **Roles** tab then press **Create New Role**
2. Create a role with the following information then **Save Role:**

   - **General Information**
   - **Role Name:** Kennon Password Reset
   - **Role Description:** Reset passwords and unlock users only
   - **User Permissions**
   - **View Users and their Details:** Check
   - **Reset Users' Passwords:** Check
   - **Unlock Users:** Check

# Image 34

### Create the Resource Set

1. Open the **Resources** tab then press **Create New Resource Set**
2. Create a resource set with the following information then **Create:**

   - **General Information**
   - **Resource Set Name:** Kennon Sales Resources
   - **Resource Set Description:** Kennon Sales group and its members
   - **Resources**
   - **Users:** Kennon Sales

# Image 35

### Assign Custom Role to Sam Patel

1. Open the **Admins** tab then press **Add Administrator**
2. Create an administrator with the following information then **Save Changes:**

   - **Admin:** Sam Patel
   - **Role:** Kennon Password Reset
   - **Resource Set:** Kennon Sales Resources

# Image 36

### Test Sam Patel on a Sales User

1. Log in to **Sam Patel's** account in an **incognito** browser
2. Open the **Directory** dropdown then select **People** then open **Casey Nguyen's** profile
3. Press **Reset or Remove Password** then **Sign Out**

# Image 37


### Verify in the System Log

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. Search for role creation events to confirm the custom role was created:
   ```text
   eventType eq "iam.role.create"
   ```
   The **Role created** event appears with your admin account as the actor, **Kennon Password Reset** as the target, and the role's permissions listed as additional targets

# Image 38
# Image 39

3. Search for resource set creation events to confirm the resource set was created:
   ```text
   eventType eq "iam.resourceset.create"
   ```
   The **Resource set created** event appears with **Kennon Sales Resources** and **Kennon Sales** as targets

# Image 40
# Image 41

4. Search for privilege changes on Sam Patel to confirm the role swap:
   ```text
   eventType sw "user.account.privilege" and target.alternateId eq "sam.patel@kennon.tech"
   ```
   A **Revoke user privilege** event removes the Read Only Administrator role, and a **Grant user privilege** event adds the custom role binding for **Kennon Password Reset**

# Image 42
# Image 43

5. Search for password resets started by Sam Patel to confirm the custom role works inside its scope:
   ```text
   eventType eq "user.account.reset_password" and actor.alternateId eq "sam.patel@kennon.tech"
   ```
   The reset event appears with **Sam Patel** as the actor and **Casey Nguyen** as the target

# Image 44
# Image 45

## 5) App Administrator Scope to One App

This phase gives Taylor the Application Administrator role limited to a single app. Taylor can manage that app without seeing other apps or managing users.

### Create Two Test Apps

1. In the **Okta Administrator Console** open the **Applications and Resources** dropdown then select **Applications**
2. Press **Browse App Catalog** and search **Bookmark App** then **Add Integration**
3. Create the first app with the following information then **Done:**

   - **Application Label:** Kennon Payroll
   - **URL:** https://example.com

# Image 46

4. Press **Back to Applications** then **Browse App Catalog** and search **Bookmark App** then **Add Integration**
5. Create the second app with the following information then **Done:**

   - **Application Label:** Kennon CRM
   - **URL:** https://example.org

# Image 47

### Assign Application Administrator to Taylor

1. Open the **Security** dropdown then **Administrators** open the **Admins** tab then press **Add Administrator**
2. Create an administrator with the following information then **Save Changes:**

   - **Admin:** Taylor Reed
   - **Role:** Application Administrator
   - **Applications:** Kennon Payroll

# Image 48

### Test Taylor on the Scoped App

1. Log in to **Okta** as **Taylor Reed** in an **incognito** browser
2. Open the **Applications and Resources** dropdown then select **Applications**
3. Confirm that **Kennon Payroll** appears and the **Kennon CRM** doesn't appear

# Image 49

4. Open **Kennon Payroll** then go to the **General** tab then press **Edit**
5. Change the **Application Label** to **Kennon Payroll Portal** then **Save**

# Image 50

6. Open the **Assignments** tab then open the **Assign** dropdown and select **Assign to People**
7. Confirm that Taylor can add assignments to people then close out of that menu

# Image 51

### Test Taylor's Limits

1. Open the **Directory** dropdown then **People**
2. Confirm that no option to create users appears

# Image 52

3. **Sign Out** of Taylor Reed's profile

### Verify in the System Log

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. Search for privilege changes on Taylor Reed to confirm the scoped app role:
   ```text
   eventType sw "user.account.privilege" and target.alternateId eq "taylor.reed@kennon.tech"
   ```
   Two **Grant user privilege** events appear at the same time, one with **Role Assigned** for the Application Administrator role and one with **Target assigned to Role** limiting it to **Kennon Payroll**

# Image 53
# Image 54

3. Search for app updates made by Taylor Reed to confirm Taylor could edit the scoped app:
   ```text
   eventType eq "application.lifecycle.update" and actor.alternateId eq "taylor.reed@kennon.tech"
   ```
   **Update application** events appear with **Taylor Reed** as the actor and **Bookmark App** as the target, from the label change to **Kennon Payroll Portal**

# Image 55
# Image 56

## 6) Assign Roles Through a Group

This phase assigns the Help Desk Administrator role to a group instead of a person, then grants and removes Riley's admin access only by changing group membership.

### Remove Riley Carter's Direct Role

1. In the **Okta Administrator Console** open the **Security** dropdown then **Administrators**
2. Open the **Admins** tab then to the right of **Riley Carter** open the **Edit** dropdown then select **Edit Assignments**
3. Press the **Trashcan** next to the **Help Desk Administrator** role then **Save Changes**

# Image 57

### Create the Admin Group

1. Open the **Directory** dropdown then **Groups** then press **Add Group**
2. Create a group with the following information then **Save:**

   - **Name:** Kennon Help Desk Admins
   - **Description:** Members receive the Help Desk Administrator role

# Image 58

### Assign the Role to a Group

1. Open the **Security** dropdown then **Adminstrators**
2. Open the **Admins** tab and press **Add Administrator**
3. Create an administrator with the following information then **Save Changes:**

   - **Admin Group:** Kennon Help Desk Admins
   - **Role:** Help Desk Administrator
   - **Groups:** Kennon Finance

# Image 59

### Add Riley Carter to the Group and Test

1. Open the **Directory** dropdown then **Groups** then select **Kennon Help Desk Admins**
2. Press **Assign People** then press the **+* next to **Riley Carter** then press **Done**

# Image 60

3. Log in to **Okta** as **Riley Carter** in an **incognito** browser
4. Open the **Directory** dropdown then select **People** then open **Avery Brooks** profile
5. Press **Resend Password Email**

# Image 61

6. **Sign Out** of Riley Carter's account

### Remove Riley from the Group

1. In the **Okta Administrator Console** open the **Securty** dropdown then **Administrators**
2. Open the **Admins** tab then under **View by Assignee** select **Groups** then open the **Kennon Help Desk Admins** group
3. Open the **People** tab then next to **Riley Carter** press the **X**

## Image 68

### Verify in the System Log

1. Open the **Reports** dropdown then select **System Log**
2. Search for group privilege grants to confirm the role was assigned to the group:
   ```text
   eventType eq "group.privilege.grant"
   ```
   Two **Grant group privilege** events appear at the same time for **Kennon Help Desk Admins**, one assigning the Help Desk Administrator role and one limiting it to **Kennon Finance**

# Image 62
# Image 63

3. Search for group membership changes on Riley Carter to confirm Riley was added and removed:
   ```text
   eventType sw "group.user_membership" and target.alternateId eq "riley.carter@kennon.tech"
   ```
   An add event and a remove event appear for **Kennon Help Desk Admins**, with your admin account as the actor

# Image 64
# Image 65

4. Search for password resets started by Riley Carter to confirm the group role worked:
   ```text
   eventType eq "user.account.reset_password" and actor.alternateId eq "riley.carter@kennon.tech"
   ```
   A reset event for **Avery Brooks** appears from while Riley's admin access came only from the group

# Image 66
# Image 67

---

## 7) Test Matrix Summary

The table below combines every admin assignment tested in this lab, showing what each test admin could and couldn't do.

| Admin | Role | Scope | Could Do | Could Not Do |
|---|---|---|---|---|
| Sam Patel | Read Only Administrator | All users and groups | View users, groups and policies | Change any setting or user |
| Riley Carter | Help Desk Administrator | All users and groups | Reset passwords for Avery and Casey | Add, deactivate or delete users |
| Riley Carter | Help Desk Administrator | Kennon Finance | Reset Avery's password | Manage Casey |
| Sam Patel | Kennon Password Reset (custom) | Kennon Sales users | Reset Casey's password | See Avery, edit or deactivate users |
| Taylor Reed | Application Administrator | Kennon Payroll | Edit the Kennon Payroll app | See Kennon CRM or security settings |
| Riley Carter | Help Desk Administrator through Kennon Help Desk Admins | Kennon Finance | Reset Avery's password while in the group | Access the Admin Console after removal |

Each admin could only act within their role and scope, showing how Okta enforces least privilege through roles, resource sets and group based assignment.

---

## 8) Cleanup

This phase removes every object created in the lab in reverse order of creation, since Okta won't delete a role, resource set or app while it's still assigned.

### Remove the Group Role Assignment

1. In the **Okta Administrator Console** open the **Security** dropdown then select **Administrators**
2. Open the **Admins** tab under **View by Assignee** select **Groups**
3. To the right of **Kennon Help Desk Admins** open the **Edit** dropdown and select **Remove Assignments**

# Image 69

4. Open the **Directory** dropdown then select **Groups** then open the **Kennon Help Desk Admins** group
5. Open the **Actions** dropdown then select **Delete**

# Image 79

### Remove the App Administrator and Test Apps

1. Under **View by Assignee** select **Users**
2. To the right of **Taylor Reed** open the **Edit** dropdown and select **Remove Individual Assignments**

# Image 70

3. Open the **Applications and Resources** dropdown then select **Applications**
4. Press the **three dots** to the right of **Kennon CRM** then select **Deactivate**
5. Press the **three dots** to the right of **Kennon Payroll Portal** then select **Deactivate**

# Image 71

6. Under **Status** select **Inactive**
7. Press the **three dots** to the right of **Kennon CRM** then select **Delete**
8. Press the **three dots** to the right of **Kennon Payroll Portal** then select **Delete**

# Image 72

### Remove the Custom Role and Resource Set

1. Open the **Security** dropdown then **Administrators** then open the **Admins** tab
2. To the right of **Sam Patel** open the **Edit** dropdown and select **Remove Individual Assignments**

# Image 73

3. Open the **Resources** tab then to the right of **Kennon Sales Resources** open the **Edit** dropdown and select **Delete Resource Set**

# Image 74

4. Open the **Roles** tab then to the right of **Kennon Password Reset** open the **Edit** dropdown then select **Delete Role**

# Image 75

### Remove the Test Users

1. Open the **Directory** dropdown then select **People** then open **Casey Nguyen's** profile
2. Open the **More Actions** dropdown then select **Deactivate**

# Image 76

3. Press the **Delete** button

# Image 77

4. Do the same with **Avery Brooks, Taylor Reed, Sam Patel, and Riley Carter**

# Image 78

### Remove the Target Groups

1. Open the **Directory** dropdown then select **Groups** then open the **Kennon Sales** group
2. Open the **Actions** dropdown then select **Delete**

# Image 80

3. Open the **Kennon Finance** group then open the **Actions** dropdown then select **Delete**

# Image 81

### Verify in the System Log

1. Open the **Reports** dropdown then select **System Log**
2. Search for privilege revocations to confirm the admin roles were removed:
   ```text
   eventType sw "user.account.privilege.revoke"
   ```
   **Revoke user privilege** events appear for **Sam Patel**, with your admin account as the actor, and **Taylor Reed**, with **Okta System** as the actor because deleting the only app in Taylor's scope revoked the role automatically

# Image 82
# Image 83

3. Search for role deletions to confirm the custom role was removed:
   ```text
   eventType eq "iam.role.delete"
   ```
   The delete event appears with **Kennon Password Reset** as the target

# Image 84
# Image 85

4. Search for resource set deletions to confirm the resource set was removed:
   ```text
   eventType eq "iam.resourceset.delete"
   ```
   The delete event appears with **Kennon Sales Resources** as the target

# Image 86
# Image 87

5. Search for application deletions to confirm both test apps were removed:
   ```text
   eventType eq "application.lifecycle.delete"
   ```
   Delete events appear for **Kennon Payroll Portal** and **Kennon CRM**, both shown as **Bookmark App**

# Image 88
# Image 89

6. Search for completed user deletions to confirm all five users were removed:
   ```text
   eventType eq "user.lifecycle.delete.completed"
   ```
   Delete events appear for **Casey Nguyen**, **Avery Brooks**, **Taylor Reed**, **Sam Patel** and **Riley Carter**

# Image 90
# Image 91

7. Search for group deletions to confirm all three groups were removed:
   ```text
   eventType eq "group.lifecycle.delete"
   ```
   Delete events appear for **Kennon Help Desk Admins**, **Kennon Sales** and **Kennon Finance**

# Image 92
# Image 93

---

## Closing Note

This lab delegated admin access for Kennon Technologies using standard roles, group scoping, a custom role with a resource set, a single app administrator, and group based role assignment. Each test admin could only act within their role and scope, so a help desk admin limited to Finance couldn't touch Sales, and an app admin couldn't see other apps or manage users.

The lab also showed a few behaviors worth remembering. A custom role with only user permissions needs the resource set to contain users, not just the group, before it can manage that group's members. Removing a user from an admin group removes their admin access right away. Deleting the only app in an Application Administrator's scope makes Okta System revoke the role automatically. Together, these show how roles, scopes and groups combine to keep admin access at least privilege.
