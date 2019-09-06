# Identifying and Remediating Host Vulnerabilities - Host Layer Round - Cleanup Phase

Now that you have completed this workshop, follow the steps below to clean up the artifacts that were created.

1.  Go to the Systems Manager Patch Manager console.

2.  Click **Configure patching**.  Click on the **Patch baselines** tab.

2.  Find the Amazon Linux 2 patch baseline that was supplied by AWS.  Click on its radio button.  From the Actions menu, click *Set default patch baseline*.  This restores the default patch baseline for Amazon Linux 2 to that which was provided by AWS.

3.  Delete the custom patch baseline for Amazon Linux 2.

4.  Go to the Amazon Inspector console.

5.  Click on the **Assessment runs** menu item and delete the runs you created.  You will be prompted for approval to delete the findings associated with the runs.

6.  Click on the **Assessment templates** menu item and delete the template you created.

7.  Click on the **Assessment targets** and delete the assessment target you created.

8.  Go to the CloudFormation console and delete the stack that you created.
