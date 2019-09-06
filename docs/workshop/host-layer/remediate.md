# Identifying and Remediating Host Vulnerabilities - Host Layer Round - Remediate Phase

In the previous Assess Phase, you installed Amazon Inspector on the instances that were launched as a result of the
CloudFormation stack.  You will now use AWS Systems Manager Patch Manager to apply patches.
You will use tags to select the instances as well.

In this section you will do the following tasks:

1. Use AWS Systems Manager Patch Manager to set up patching
2. Use AWS Systems Manager Run Command to check the status of the patching

## Use AWS Systems Manager Patch Manager

1. Go to the Systems Manager console and select Patch Manager.  If you see the Patch Manager home screen, then click the **View predefined patch baselines** link as shown below:

    ![remediate-home-screen](./images/remediate-pm-home-screen.png)

2.  You will now see a list of default patch baselines that are used to patch each operating system supported by Patch Manager.   The default patch baseline only patches major security issues.  You are going to crate a new Amazon Linux 2 patch baseline that will patch more things and make this new patch baseline the default.  Click **Create patch baseline**.


3.  In the *Name* field, enter a name to give the new baseline such as **pww**.  In the *Operating system* field, select **Amazon Linux 2**.  In the *Approval rules* section, check the box under *Include non-security updates*.  IMPORTANT NOTE:  Depending on the size of the screen, the box may not align with the title *Include non-security updates*.  See the figure below.

    ![approval-rule](./images/remediate-pm-approval-rule.png)

4.  Click the **Create patch baseline** button at the bottom of the screen.  You should now see the new patch baseline in the list of baselines.  You may need to refresh the browser window to see it.  This new patch baseline includes non-security patches.  Note at the end of the line representing the newly created patch baseline you will see *No* in the *Default Baseline* column as shown in the figure below.

    ![default-baseline](./images/remediate-pm-default-baseline-no.png)

5. Click the radio button on the line with the newly created patch baseline.  From the *Actions* menu at the top select **Set default patch baseline**.  You will be asked to confirm this.  You have just set the default patch baseline for Amazon Linux 2 to use the patch baseline you just created that includes non-security patches.  You should now see *Yes* at the end of the patch baseline as shown below.

    ![default-baseline](./images/remediate-pm-default-baseline-yes.png)


6. Click **Configure patching**. In the *Configure patching* screen, go to the *Instances to patch* section and click the **Enter instance tags** radio button.  In the *Instance tags* field, enter *aws:cloudformation:stack-name* into the *Tag key* field.  In the *Tag value* field, enter the stack name that you created earlier (pww, following the example in this documentation).  Click **Add**.


7. In the *Patching schedule* section, click the *Skip scheduling and patch instances now* radio button.

8. In the *Patching operation* section, click the *Scan and install* radio button if it is not already selected.  Your screen should look similar to the image below.

    ![configure-patching](./images/remediate-pm-configure-patching.png)

9.  Click the **Configure patching** button at the bottom of the window.  You will see a message at the top of your screen saying that *Patch Manager* will use *Run Command* to patch the instances.   *Run Command* is another feature of AWS Systems Manager that runs a command across multiple Amazon EC2 instances.  Patch Manager builds the commands necessary to perform the patching and is using Run Command to actually execute the commands.

## Check the patching status

You are now going to examine the status of the patching operation by using AWS Systems Manager Run Command.

1. Go to the AWS Systems Manager Console and click **Run Command** on the left menu.  If the patching is still running, you will see the entry in the *Commands* tab.  Wait for the command to finish.  Refresh the screen if necessary to update the display.  Once the command has finished, click on **Command history**.

2. Look for the line containing the document name **AWS-RunPatchBaseline**.  That represents the Patch Manager activity.Your screen should look similar to the image below.


    ![command-history](./images/remediate-command-history.png)

3.  Click on the **Command ID** link to see more details about the command.  You will see a line for each target with a link referencing the Instance ID.  If you then click on the **Instance ID**, you will see each step of the command that is executed.   Note that some steps are skipped because they do not apply to the operating system of the instance.  Also, you only see the first part of the command output.  If you want to see all of the output you can configure Systems Manager to direct the output into an Amazon S3 bucket.

    You have now completed the patching operation.  In the Verify Phase, you will re-assess the environment with Amazon Inspector.

Click [here](./verify.md) to proceed to the Verify Phase.
