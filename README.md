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

###Install apex-lang Package
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

##Prepare for Deployment

###Install Apache Ant

Install Apache Ant on your workstation - manual installation instructions can be found at http://tutorialforlinux.com/2014/03/04/how-to-install-apache-ant-on-mac-mavericks-10-9-os-x-step-by-step-easy-guide/, or you can install using Homebrew (http://brew.sh/) by running the install command:

    brew install ant

###Install Force.com Migration Tool

Install the Force.com Migration Tool by following instructions found at http://www.salesforce.com/us/developer/docs/daas/Content/forcemigrationtool_install.htm.

###Configure Force.com Migration Tool

#####Credentials (build.properties)

In the **Samples** folder of the Force.com Migration Tool directory, locate the build.properties file. Update this file to match the Lampo Salesforce development account credentials. *NOTE: You must append the Salesforce.com security key to the end of the password in the configuration file.*

#####Ant Configuration (build.xml)

Add the following XML to the **build.xml** file.

    <!-- The file lampo/package.xml lists what is to be retrieved -->
    <target name="retrieveELP">
      <mkdir dir="ELP"/>
      <!-- Retrieve the contents into another directory -->
      <sf:retrieve username="${sf.username}" password="${sf.password}" 
        serverurl="${sf.serverurl}" maxPoll="${sf.maxPoll}" retrieveTarget="ELP" 
        unpackaged="ELP/package.xml"/>
    </target>
    <!-- Deploy the Lampo ELP metadata to Salesforce Org -->
    <target name="deployELP">
      <sf:deploy username="${sf.username}" password="${sf.password}" 
        serverurl="${sf.serverurl}" maxPoll="${sf.maxPoll}" deployRoot="ELP" 
        rollbackOnError="true" ignoreWarnings="true"/>
    </target>
    
#####Package Configuration (Package.xml)

Create a new folder in the **Sample** directory named **"ELP".**

In the **ELP** directory, create a new file called **"package.xml".**

Set the contents of the **package.xml** file to the following XML:

```
<?xml version="1.0" encoding="UTF-8"?>
<Package xmlns="http://soap.sforce.com/2006/04/metadata">
    <types>
        <members>*</members>
        <name>AccountOwnerSharingRule</name>
    </types>
    <types>
        <members>AccountHierarchyTestData</members>
        <members>AccountStructure</members>
        <members>BatchFieldUpdate</members>
        <members>Component_Query</members>
        <members>Contactsextension</members>
        <members>ElpAccountActivities</members>
        <members>ElpAccountTerritoryMap</members>
        <members>ElpAddProperty</members>
        <members>ElpApplicantActivities</members>
        <members>ElpApplicantController</members>
        <members>ElpApplicantControllerTests</members>
        <members>ElpApplicantDetails</members>
        <members>ElpApplicantHireController</members>
        <members>ElpApplicantImportController</members>
        <members>ElpApplicantResendNotification</members>
        <members>ElpApplicantScheduledTasks</members>
        <members>ElpApplicantSearch</members>
        <members>ElpApplicantSearchController</members>
        <members>ElpApplicantService</members>
        <members>ElpApplicantTests</members>
        <members>ElpContactActivities</members>
        <members>ElpContactActivitiesTest</members>
        <members>ElpDemoDataCreator</members>
        <members>ElpFlagBadEmails</members>
        <members>ElpFlagBadEmailsTest</members>
        <members>ElpGeneralIssuesTests</members>
        <members>ElpGlobals</members>
        <members>ElpMasterMap</members>
        <members>ElpMetadataTriggerTests</members>
        <members>ElpOpportunityActivities</members>
        <members>ElpOpportunityActivitiesTest</members>
        <members>ElpOpportunityAssignment</members>
        <members>ElpOpportunitySearch</members>
        <members>ElpRealEstateOpportunityAssignmentTest</members>
        <members>ElpRegistration</members>
        <members>ElpRelatedOpportunities</members>
        <members>ElpSystemCheck</members>
        <members>ElpTerritoryContactRequirementTests</members>
        <members>ElpTerritoryEdit</members>
        <members>ElpTestingHelper</members>
        <members>ElpTests</members>
        <members>ExternalKeyGenerator</members>
        <members>GroupsHelper</members>
        <members>InlineAcountHerachy_TestUtilities</members>
        <members>Metaphone</members>
        <members>OpenStreetMapGeocoder</members>
        <members>PickListDescController</members>
        <members>PicklistDescriber</members>
        <members>RoundRobinAssignment</members>
        <members>RoundRobinAssignmentTests</members>
        <members>ShowPicture</members>
        <members>TestUtilities</members>
        <members>testAccountHierarchy</members>
        <members>testMetaphone</members>
        <name>ApexClass</name>
    </types>
    <types>
        <members>AccountHierarchyTree</members>
        <members>MapquestMap</members>
        <members>Query</members>
        <name>ApexComponent</name>
    </types>
    <types>
        <members>AccountHierarchyPage</members>
        <members>AccountImageUpload</members>
        <members>ElpAccountTerritoryMap</members>
        <members>ElpAddProperty</members>
        <members>ElpApplicantDetails</members>
        <members>ElpApplicantDetailsChangeStatus</members>
        <members>ElpApplicantExportToExcel</members>
        <members>ElpApplicantSearch</members>
        <members>ElpFlagBadEmails</members>
        <members>ElpOpportunityAssignment</members>
        <members>ElpOpportunitySearch</members>
        <members>ElpRegistration</members>
        <members>ElpRelatedOpportunities</members>
        <members>ElpSystemCheck</members>
        <members>ElpTerritoryEdit</members>
        <members>MqMap</members>
        <members>PicklistDesc</members>
        <members>showPicture</members>
        <name>ApexPage</name>
    </types>
    <types>
        <members>*</members>
        <name>ApexTrigger</name>
    </types>
    <types>
        <members>Opportunity.ELP Applicant</members>
        <members>Opportunity.ELP Financial Referral</members>
        <members>Opportunity.ELP Real Estate</members>
        <name>BusinessProcess</name>
    </types>
    <types>
        <members>ELPProgramAdmin</members>
        <members>ELP_Applicants</members>
        <members>ElpAdvisor</members>
        <name>CustomApplication</name>
    </types>
    <types>
        <members>Account.Active__c</members>
        <members>Account.CanReceiveReferrals__c</members>
        <members>Account.ElpAccountId__c</members>
        <members>Account.ExternalKey__c</members>
        <members>Account.ForUnassigned__c</members>
        <members>Account.LanguagesServed__c</members>
        <members>Account.Orphan__c</members>
        <members>Account.PhotoUrl__c</members>
        <members>Account.Photo__c</members>
        <members>Account.TerritoryPostalCodes__c</members>
        <members>Contact.MailingLatitude__c</members>
        <members>Contact.MailingLongitude__c</members>
        <members>Contact.MobileCarrier__c</members>
        <members>Contact.NameEmailHash__c</members>
        <members>Contact.NotificationMethods__c</members>
        <members>Contact.ReferralRequestID__c</members>
        <members>Opportunity.ActiveApplication__c</members>
        <members>Opportunity.ApplicantCity__c</members>
        <members>Opportunity.ApplicantPostal__c</members>
        <members>Opportunity.ApplicantState__c</members>
        <members>Opportunity.AssignmentDate__c</members>
        <members>Opportunity.AssignmentID__c</members>
        <members>Opportunity.BrokerEmailAddress__c</members>
        <members>Opportunity.ContextBrandSpecialty__c</members>
        <members>Opportunity.CurrentMYTD__c</members>
        <members>Opportunity.Current_YTD_Count__c</members>
        <members>Opportunity.Current_YTD__c</members>
        <members>Opportunity.ElpDepartment__c</members>
        <members>Opportunity.Email__c</members>
        <members>Opportunity.ExportedAt__c</members>
        <members>Opportunity.FirstName__c</members>
        <members>Opportunity.LastName__c</members>
        <members>Opportunity.LastRequestForUpdate__c</members>
        <members>Opportunity.MarketName__c</members>
        <members>Opportunity.NotificationsNeeded__c</members>
        <members>Opportunity.PartnerCommission__c</members>
        <members>Opportunity.Payee__c</members>
        <members>Opportunity.Previous_MTD__c</members>
        <members>Opportunity.Previous_MYTD__c</members>
        <members>Opportunity.Previous_Month_TD_Count__c</members>
        <members>Opportunity.Previous_YTD_Count__c</members>
        <members>Opportunity.PrimaryContactEmailBouncedDate__c</members>
        <members>Opportunity.PrimaryContact__c</members>
        <members>Opportunity.PropertyCity__c</members>
        <members>Opportunity.PropertyIsCurrentlyListed__c</members>
        <members>Opportunity.PropertyIsMobileHome__c</members>
        <members>Opportunity.PropertyPostalCode__c</members>
        <members>Opportunity.PropertyPriceRange__c</members>
        <members>Opportunity.PropertyReason__c</members>
        <members>Opportunity.PropertyRole__c</members>
        <members>Opportunity.PropertyState__c</members>
        <members>Opportunity.PropertyStreet__c</members>
        <members>Opportunity.PropertyTimeframe__c</members>
        <members>Opportunity.RecordTypeName__c</members>
        <members>Opportunity.ReferralPercentage__c</members>
        <members>Opportunity.ReferralRequestID__c</members>
        <members>Opportunity.ResetNotifications__c</members>
        <members>Opportunity.SelectedBrandSpecialtyName__c</members>
        <members>Opportunity.SelectedBrandSpecialty__c</members>
        <members>Opportunity.StatsSubmittedMonth__c</members>
        <members>Opportunity.StatsSubmittedYear__c</members>
        <members>Opportunity.SubmittedDate__c</members>
        <members>Opportunity.SystemQualificationLastExecutedAt__c</members>
        <members>Opportunity.TransactionValue__c</members>
        <members>Opportunity.WelcomeEmailSentAt__c</members>
        <members>User.AdminAssistant__c</members>
        <name>CustomField</name>
    </types>
    <types>
        <members>*</members>
        <name>CustomObject</name>
    </types>
    <types>
        <members>*</members>
        <name>CustomTab</name>
    </types>
    <types>
        <members>ELP</members>
        <members>ELP/ELP_Email_Letterhead.jpg</members>
        <name>Document</name>
    </types>
    <types>
        <members>ELP</members>
        <members>ELP/ApplicantOwnerChange</members>
        <members>ELP/Applicant_Link_to_Application</members>
        <members>ELP/Applicant_Request_for_Update</members>
        <members>ELP/Applicant_Simple_Form_Followup</members>
        <members>ELP/Applicant_Welcome</members>
        <members>ELP/ELP_Applicant_Import</members>
        <members>ELP/PLRNewLeadNotification</members>
        <name>EmailTemplate</name>
    </types>
    <types>
        <members>*</members>
        <name>Layout</name>
    </types>
    <types>
        <members>ELP_Applicant</members>
        <name>Letterhead</name>
    </types>
    <types>
        <members>Account.AllAccounts</members>
        <members>Account.ELP_Territories</members>
        <members>Account.Elp_Parent_Accounts</members>
        <members>Account.Territories</members>
        <members>Opportunity.Elp_Opportunities</members>
        <name>ListView</name>
    </types>
    <types>
        <members>Account.ElpCompany</members>
        <members>Account.ElpTerritory</members>
        <members>Contact.Customer</members>
        <members>Contact.ELPApplicant</members>
        <members>Contact.ElpPrivate</members>
        <members>Contact.ElpPublic</members>
        <members>Contact.ElpUser</members>
        <members>Opportunity.ELP_Applicant</members>
        <members>Opportunity.ELP_Financial_Referral</members>
        <members>Opportunity.ELP_Real_Estate_Referral</members>
        <name>RecordType</name>
    </types>
    <types>
        <members>*</members>
        <name>Role</name>
    </types>
    <types>
        <members>ElpMapping</members>
        <members>PictureUploader</members>
        <name>StaticResource</name>
    </types>
    <types>
        <members>Account.TerritoryCannotBeActiveAndOrphan</members>
        <members>Account.TerritoryZipCodesCsv</members>
        <members>Contact.ElpContactMustHave2CharMailState</members>
        <members>Contact.ElpContactMustHave2CharOtherState</members>
        <members>Contact.ElpContactMustHaveEmailAddress</members>
        <members>Opportunity.ELP_Records_require_Account</members>
        <members>Opportunity.ELP_Records_require_a_BrandSpecialty</members>
        <name>ValidationRule</name>
    </types>
    <types>
        <members>Opportunity.ApplicantChangeStatus</members>
        <members>Opportunity.Assign</members>
        <members>Opportunity.ElpRelatedOpportunities</members>
        <members>Opportunity.NewProperty</members>
        <members>Opportunity.RequestUpdateBulk</members>
        <members>Opportunity.RequestUpdateSingle</members>
        <name>WebLink</name>
    </types>
    <types>
        <members>*</members>
        <name>WorkflowAlert</name>
    </types>
    <types>
        <members>*</members>
        <name>WorkflowFieldUpdate</name>
    </types>
    <types>
        <members>Opportunity.Applicant%3A Continue Application</members>
        <members>Opportunity.Applicant%3A Import</members>
        <members>Opportunity.Applicant%3A Periodic Update</members>
        <members>Opportunity.Applicant%3A Request Link</members>
        <members>Opportunity.Applicant%3A Request Update</members>
        <members>Opportunity.Applicant%3A Welcome</members>
        <name>WorkflowRule</name>
    </types>
    <version>31.0</version>
</Package>
```

##Using Force.com Migration Tool

###Retrieve ELP Code from Lampo Development Account

Open a Terminal window and navigate to the Force.com Migration Tool's **Samples** folder.

Execute the following command:

    ant retrieveELP

The ELP folder will now contain the custom code downloaded from the Lampo development account.

###Deploy ELP Code to New Developer Account

While still in the terminal in the **Samples** folder, execute the following command:

    nano build.properties

Update the credentials in this file to match the credentials created for your new Salesforce.com developer account.

Execute the following command:

    ant deployELP

This command deploys the custom code to your Salesforce.com Developer organization.
