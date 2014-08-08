Deploying to new Developer Account
==================================

The following instructions will help deploy the ELP code to a new developer account.

##Set up organization

Create a new development account at https://developer.salesforce.com/signup

###Organization Wide Defaults

From Setup, click **Security Controls | Sharing Settings.**
Click Edit in the Organization-Wide Defaults area.

Update Org Wide Defaults to match the following:

Object	|	Default Internal Access	|	Default External Access	|	Grant Access Using Hierarchies
------  | ----------------------- | ----------------------- | ------------------------------
Lead	|	Public Read/Write/Transfer	|	Public Read/Write/Transfer	|	Yes
Account, Contract and Asset	|	Public Read/Write	|	Public Read/Write	|	Yes
Contact	|	Public Read Only	|	Public Read Only	|	Yes
Opportunity	|	Private	|	Private	|	Yes
Quote	|	Controlled by Parent	|	Controlled by Parent	|	Yes
Case	|	Private	|	Private	|	Yes
Campaign	|	Public Full Access	|	Public Full Access	|	Yes
User	|	Public Read Only	|	Private	|	Yes
Activity	|	Private	|	Private	|	Yes
Calendar	|	Hide Details and Add Events	|	Hide Details and Add Events	|	Yes
Price Book	|	Use	|	Use	|	Yes
Account Brand Specialty	|	Controlled by Parent	|	Controlled by Parent	|	No
Applicant Brand Specialty	|	Controlled by Parent	|	Controlled by Parent	|	No
Application	|	Controlled by Parent	|	Controlled by Parent	|	No
Assignment Group	|	Public Read/Write	|	Public Read/Write	|	Yes
Assignment Group Member	|	Controlled by Parent	|	Controlled by Parent	|	No
Brand	|	Public Read Only	|	Public Read Only	|	Yes
Brand Specialty	|	Public Read Only	|	Public Read Only	|	Yes
Elp Program	|	Public Read Only	|	Public Read Only	|	Yes
Elp Specialty	|	Public Read Only	|	Public Read Only	|	Yes
Partner Card	|	Controlled by Parent	|	Controlled by Parent	|	No

###Enable History Tracking on Opportunity, Account, & Contact

**To set up field history tracking:**

1. From Setup, click **Customize.**
2. Select the object you want to configure.
3. Click **Fields | Set History** Tracking.

###Add Organization Wide Email Address
From Setup, click **Email Administration | Organization-Wide Addresses.**

Click **Add** to create a new organization-wide address. 

The workflows in the package are configured to use the email address elp.noreply@daveramsey.com, but you may choose any email (you will need to verify the email address). If you choose a different address, you'll have to update the .workflow files in the project to match the Organization Wide Email address you configured.

###Install & Update apex-lang package
Install the apex-lang package from https://code.google.com/p/apex-lang/. Choose **Production as Managed Package.**

###Enable Partner Portal
**To enable the partner portal:**

1. From Setup, click **Customize | Partners | Settings.**
2. Click **Edit.**
3. Select the **Enable Partner Portal** checkbox.
4. Click **Save.**

###Enable Quotes
**To enable Quotes for your organization:**

1. From Setup, click **Customize | Quotes | Settings.**
2. Select **Enable Quotes.**
3. Click **Save.**
4. Select *Opportunity Layout* to display the Quotes related list on the standard opportunity page layout.
5. Optionally, select *Add to users' customized related lists* to add the Quotes related list to all opportunity page layouts users have customized.
6. Click **Save** to finish.

###Create ELP folder in Documents

1. From Salesforce, click the **+** tab to view all available tabs.
2. Click **Documents.**
3. In the *Document Folders* section, click the **Create New Folder** link.
4. Name the new folder "ELP", and click **Save.**

###Create ELP folder in Communication Templates

1. From setup, click **Communication Templates.**
2. Click the **Create New Folder** link.
3. Name the folder "ELP", and click **Save.**

###Update Workflow Email (if not elp.noreply@daveramsey.com)

If you set your Organization Wide Email Address to something other than "elp.noreply@daveramsey.com", then open the workflows/Opportunity.workflow file, find & replace the email address "elp.noreply@daveramsey.com" with the email address you set as your Organization Wide Email Address.
