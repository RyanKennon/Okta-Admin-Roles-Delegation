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



























