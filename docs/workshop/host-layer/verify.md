# Identifying and Remediating Host Vulnerabilities - Host Layer Round - Verify Phase

Now that you have remediate the environment, you will again use Amazon Inspector to assess the environment again to see how the patching affected the overall security posture of the environment.

1.  Go to the Amazon Inspector console, click **Assessment templates** on the menu.

2.  Locate the template that you created during the Assess Phase and check the box at the left end of that row.

3.  Click **Run**.  This will launch another assessment run.

4.  Click **Assessment runs** and periodically refresh the screen.  Wait until the status for the run changes to *Analysis complete*.  The run will take approximately 15 minutes to complete.

5.  Compare the number of findings between the two runs.   In most cases, there will be fewer findings in the newer run since patches have been applied.   The change in the number findings may vary based on the age of the AMI used to launch the instances.

7.  Click the number of findings for the newest run (after the patches were installed).  You will then see all of the findings that were not patched during the Remediate Phase.

8.  Take a look at the entries that were not patched.  A common example of a finding is an instance is configured to allow users to log in with root credentials over SSH, without having to use a command authenticated by a public key.  Why would Patch Manager not patch this or the other findings?

9.  You have now completed this round.  Click [here](./cleanup.md) to proceed to the Cleanup Phase.
