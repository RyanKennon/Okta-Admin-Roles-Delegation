# Okta-Admin-Roles-Delegation

## 1) Prepare Users and Groups

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
2. In the **Search** bar use the following function:
   `eventType eq "user.lifecycle.create"`

# Image 13
# Image 14

3. In the **Search** bar use the following function:
   `eventType eq "user.mfa.factor.activate"`

# Image 15
# Image 16

---

## 2) Standard Admin Roles

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

### Verify in the System Logs

1. In the **Okta Administrator Console** open the **Reports** dropdown  then **System Log**
2. In the **Search** function enter the following code:
   `eventType eq "user.account.privilege.grant"`

# Image 23
# Image 24

3. In the **Search** function enter the following code:
   `eventType eq "user.account.reset_password" and actor.alternateId eq "riley.carter@kennon.tech"`

# Image 25
# Image 26

---

## 3) Scoped Delegation with a Standard Role

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
2. In the **Search** function enter the following code:
   `eventType sw "user.account.privilege" and target.alternateId eq "riley.carter@kennon.tech"`

# Image 29
# Image 30

3. In the **Search** function enter the following code:
   `eventType eq "user.account.reset_password" and actor.alternateId eq "riley.carter@kennon.tech"`

# Image 31
# Image 32

## 4) Custom Admin Role and Resource Set

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


### Verify in the System Logs

1. In the **Okta Administrator Console** open the **Reports** dropdown then **System Log**
2. In the **Search** function enter the following code:
   `eventType eq "iam.role.create"`

# Image 38
# Image 39

3. In the **Search** function enter the following code:
   `eventType eq "iam.resourceset.create"`

# Image 40
# Image 41

4. In the **Search** function enter the following code:
   `eventType sw "user.account.privilege" and target.alternateId eq "sam.patel@kennon.tech"`

# Image 42
# Image 43

5. In the **Search** function enter the following code:
   `eventType eq "user.account.reset_password" and actor.alternateId eq "sam.patel@kennon.tech"`

# Image 44
# Image 45

## 5) App Administrator Scope to One App



















