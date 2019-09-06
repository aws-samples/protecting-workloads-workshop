# Supercharging your Workload Defenses - Build Phase

## Environment setup

To setup the workshop environment, launch the CloudFormation stack below in the preferred AWS region using the "Deploy to AWS" links below. This will automatically take you to the console to run the template.

!!! Attention
    <p style="font-size:16px;">
      For participants in the re:Inforce Builder Session, _FND305 - Supercharging your workload defenses with AWS WAF, Amazon Inspector, and AWS Systems Manager_, **you do not need to deploy the CloudFormation Stack as it has already been created. Follow the instructions in the drop down directly below for _Event Engine_.**
    </p>

??? info  "Click here if you're at an *AWS event* where the *Event Engine* is being used" 
	1. Navigate to the <a href="https://dashboard.eventengine.run" target="_blank">Event Engine dashboard</a>
	2. Enter your **team hash** code. 
	3. Click **AWS Console**
    4. Make sure you are in the correct region.
    5. Browse to the CloudFormation Console.
	6. Move on to **[Assess phase](assess.md)**.

---

!!! info "Note About Workshop and AWS Account"
    <p style="font-size:16px;">
    _If you are not using an AWS account provided by the Workshop Team, we **strongly recommend that you use a non-production AWS account for this workshop** such as a training, sandbox or personal account. If multiple participants are sharing a single account you should use unique names for the stack set and resources created in the console._
    </p>

If you are using your own account or an account provided by the Workshop Team, proceed with CloudFormation deployment in the preferred AWS region below:

**US East 2 (Ohio)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-builder.yml" target="_blank">![Deploy in us-east-2](/images/deploy-to-aws.png)</a>

---

**US West 2 (Oregon)** &nbsp; &nbsp; &nbsp; &nbsp; 
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-builder.yml" target="_blank">![Deploy in us-west-2](/images/deploy-to-aws.png)</a>

---

**US East 1 (N. Virginia)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-builder.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**EU West 1 (Ireland)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-builder.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**AP Southeast 2 (Sydney)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-builder.yml" target="_blank">![Deploy in ap-southeast-2](/images/deploy-to-aws.png)</a>

---

1. Click ***Next*** on the Specify Template section.

2. On the Specify stack details step, update the following parameters depending on how you are doing this workshop:

- If you are sharing an AWS account with someone else in the same region, change the name of the stack to __pww-yourinitials__
- Trusted Network CIDR: Enter a trusted IP or CIDR range you will access the site from using a web browser. You can obtain your current IP at <a href="https://ifconfig.co/" target="_blank">Ifconfig.co</a> The entry should follow CIDR notation. i.e. 10.10.10.10/32 for a single host.
- Keep the defaults for the rest of the parameters.

3. Click ***Next***

4. Click Next on the ***Configure stack options*** section.

5. Finally, acknowledge that the template will create IAM roles under Capabilities and click **Create**.

This will bring you back to the CloudFormation console. You can refresh the page to see the stack starting to create. Before moving on, make sure the stack is in a __CREATE_COMPLETE__ status. This should take ~8 minutes.

---

Click [here](assess.md) to proceed to the Assess Phase.
