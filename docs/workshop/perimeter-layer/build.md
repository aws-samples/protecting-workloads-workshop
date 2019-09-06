# Mitigating Common Web Application Attack Vectors Using AWS WAF - Build Phase

## Environment setup

To setup the workshop environment, launch the CloudFormation stack below in the preferred AWS region using the "Deploy to AWS" links below. This will automatically take you to the console to run the template.

??? info  "Click here if you're at an *AWS event* where the *Event Engine* is being used" 

    _The CloudFormation Stack for this event should already be deployed._

	1. Click <a href="https://dashboard.eventengine.run" target="_blank">here</a> to open the Event Engine dashboard in a separate browser tab.
	2. Enter the **team hash** code that you were provided and click **Proceed**.. 
	3. Click **AWS Console**.
	4. Click **Open Console**.
	5. Make sure you are in the correct region.
	6. Go to the CloudFormation console.
	7. Click **[here](assess.md)** to proceed to the Assess phase.

---

??? info  "Click here if you're *not at an AWS event* or are using your own account" 

    In order to complete these workshops, you'll need a valid, usable <a href="https://aws.amazon.com/getting-started/" target="_blank">AWS Account</a>. Use a personal account or create a new AWS account to ensure you have the necessary access and that you do not accidentally modify corporate resources. Do **not** use an AWS account from the company you work for. __We strongly recommend that you use a non-production AWS account for this workshop such as a training, sandbox or personal account. If multiple participants are sharing a single account you should use unique names for the stack set and resources created in the console.__

---

**US West 2 (Oregon)** &nbsp; &nbsp; &nbsp; &nbsp; 
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/workshoptest/pww-workshop-env-build.yml" target="_blank">![Deploy in us-west-2](/images/deploy-to-aws.png)</a>

---

**US East 2 (Ohio)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/workshoptest/pww-workshop-env-build.yml" target="_blank">![Deploy in us-east-2](/images/deploy-to-aws.png)</a>

---

**US East 1 (N. Virginia)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/workshoptest/pww-workshop-env-build.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**EU West 1 (Ireland)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/workshoptest/pww-workshop-env-build.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**AP Southeast 2 (Sydney)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/workshoptest/pww-workshop-env-build.yml" target="_blank">![Deploy in ap-southeast-2](/images/deploy-to-aws.png)</a>

---

1. Click ***Next*** on the Specify Template section.

2. On the Specify stack details step, update the following parameters depending on how you are doing this workshop:

??? info "Individual or an event not sponsored by AWS"

    - If you are sharing an AWS account with someone else in the same region, change the name of the stack to __pww-yourinitials__
    - Automated Scanner: __Set to false.__
    - Scanner Username: Enter __null__
    - Scanner Password: Enter __null__
    - Trusted Network CIDR: Enter a trusted IP or CIDR range you will access the site from using a web browser. You can obtain your current IP at <a href="https://ifconfig.co/" target="_blank">Ifconfig.co</a> The entry should follow CIDR notation. i.e. 10.10.10.10/32 for a single host.
    - Keep the defaults for the rest of the parameters.

3. Click ***Next*** 

4. Click Next on the ***Configure stack options*** section.

5. Finally, acknowledge that the template will create IAM roles under Capabilities and click **Create**.

This will bring you back to the CloudFormation console. You can refresh the page to see the stack starting to create. Before moving on, make sure the stack is in a __CREATE_COMPLETE__ status. This should take ~8 minutes.

---

You can now proceed to the [Assess Phase](assess.md).
