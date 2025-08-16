# Jenkins Email Notification for Failed Builds

This guide explains how to configure Jenkins to send **email alerts** when a build fails. We’ll use Gmail with an App Password to send notifications.

---

## Prerequisites

* Jenkins server installed and running
* A Gmail account to act as the sender (`email-a@gmail.com`)
* Another email to receive alerts (`email-b@gmail.com`)
* **Google App Password** (not your Gmail login password)

---

## Step 1 — Generate Google App Password

1. Go to your Gmail account → **Manage Google Account**
2. Navigate to **Security** → enable **2-Step Verification**
3. Under **App passwords**, create a new password for **Mail**
4. Copy the 16-character generated password (e.g., `abcd efgh ijkl mnop`)

![](/jennkins-email-img/app-passwd-generator.png)

This will be used in Jenkins instead of your Gmail password.

---

## Step 2 — Configure Jenkins Email Settings

1. Go to **Manage Jenkins → Configure System → Extended E-mail Notification**

   * SMTP server: `smtp.gmail.com`
   * SMTP port: `465`
   * Use SSL: ✅
   * Authentication: ✅

     * Username: `email-a@gmail.com`
     * Password: *App password generated above*
     
   ![](/jennkins-email-img/sys-email-config-1.png)

Click **Test configuration** → send a test email.

![](/jennkins-email-img/test-email.png)

![](/jennkins-email-img/test-email-1.png)

---

## Step 3 — Create a Jenkins Job

1. Go to **New Item → Freestyle Project**
2. Under **Source Code Management**, choose **Git**

   * Repo URL: `https://github.com/<your-repo>.git`
   * Branch: `master` (❌ incorrect if repo uses `main`)
3. Save and run the job → it will **fail** since the branch is wrong.

---

## Step 4 — Configure Email on Build Failure

1. In the job configuration → **Post-build Actions**
2. Add **Editable Email Notification**
3. Configure:

   * Project Recipient List: `email-b@gmail.com`
   * Triggers: ✅ `Failure - Any` (send only when build fails)
   * Content: default or custom message

Save the job.

![](/jennkins-email-img/wrong-job-1.png)

![](/jennkins-email-img/wrong-job-2.png)

4. Assigning Reciever-side-email

![](/jennkins-email-img/reciever-email.png)

---

## Test the Setup

1. Run the job with the wrong branch (`master` instead of `main`)
2. The job fails
3. Jenkins sends an email to `email-b@gmail.com` with failure details

![](/jennkins-email-img/build-fail.png)



---

Now Jenkins will notify you via email whenever a build fails!

![](/jennkins-email-img/final-output.png)
