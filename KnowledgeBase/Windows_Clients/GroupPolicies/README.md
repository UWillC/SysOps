# Windows 10 Group Policies

## Delete roaming and local profiles

This policy setting allows an administrator to automatically delete user profiles on system restart that have not been used within a specified number of days.

If you enable this policy setting, the User Profile Service will automatically delete on the next system restart all user profiles on the computer that have not been used within the specified number of days.

**SOLUTION**
Navigate to Computer Configuration > Administrative Templates > System > User Profiles and double-click the policy of "Delete user profiles older than a specified number of days on system restart". Set it to Enabled and specify the number of days.

When a user account is deleted, all its data will be removed, including their desktop, downloads, documents, photos, music, and their folder inside C:\Users.

The deletion of an account causes a log-entry to be written to the Event Log with Event ID 4726. This event can trigger a script that you may define, if required, in the Task Scheduler for doing further cleanup.

## Install Google Chrome extension for all users

Deploying extensions via Group Policy consists of two parts:

1. Retrieve the extension ID and the update URL of the Chrome extension
2. Enable and configure Chrome extensions in a Group Policy

**SOLUTION**

### Retrieve the extension ID and update URL of the Chrome extension

The first thing to do, is to manually install the extension directly from the Chrome Web Store on your (test) system. You need to do this, otherwise you will not be able to retrieve the ID and update URL.

The extension ID can be retrieved by opening the extensions tab in Chrome. Either enter chrome://extensions in the address bar or open the extensions tab via the menu.

Enable developer mode. Now the ID of each individual extension is shown.

Copy this ID somewhere (for example in Notepad); you will need this information in the next step.

*ndjpnladcallmjemlbaebfadecfhkepb*

Chrome extensions are installed on a per-user basis. The installation directory is:

*C:\Users\%UserName%\AppData\Local\Google\Chrome\User Data\Default\Extensions*

The extension ID is equal to the name of the folder. Open the directory that corresponds with the ID of your extension, in our case ndjpnladcallmjemlbaebfadecfhkepb (= the ID of the Office Online extension). Open the subdirectory representing the version of the extension. In the root of this directory you should find the file manifest.json. Open this file in your favorite text editor (e.g. Notepad). Search for the string update_url. Here you will find the update URL.

*https://clients2.google.com/service/update2/crx*

Now you have the values you need. Copy them together in one string and make sure to separate them using a semicolon (as shown in the beginning of this paragraph):

**ndjpnladcallmjemlbaebfadecfhkepb;https://clients2.google.com/service/update2/crx**

### Configure the Group Policy setting to deploy the Chrome extension

Before you continue reading, please make sure that you have imported the Google Chrome ADMX files in your environment.

To force-install extensions, open your Group Policy Management console (gpmc.msc) and go to User Configuration \ Administrative Templates \ Google\  Google Chrome \ Extensions. Go to the setting Configure the list of force-installed apps and extensions and enable it.

Click the Show button and enter the string you created in the previous paragraph:

*ndjpnladcallmjemlbaebfadecfhkepb;https://clients2.google.com/service/update2/crx*

Now the policy setting is configured. On the next Group Policy refresh the user will automatically receive the required extension. To summarize, this policy will automatically install one or more extensions for all users to whom the Group Policy applies. The installation is executed silently and without user interaction.