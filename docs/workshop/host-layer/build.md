# Identifying and Remediating Host Vulnerabilities - Host Layer Round - Build Phase

## Environment setup

!!! info "For those who have completed the Web Application Firewall round"
    __If you have completed the Web Application Firewall round, this round uses the same environment. If you have not deleted the AWS CloudFormation stack from the Web Application Firewall round, you can click [here](./assess.md) to proceed to the Assess Phase.__
    
---

??? info  "Click here if you're *not at an AWS event* or are using your own account"

    In order to complete these workshops, you'll need a valid, usable <a href="https://aws.amazon.com/getting-started/" target="_blank">AWS Account</a>. Use a personal account or create a new AWS account to ensure you have the necessary access and that you do not accidentally modify corporate resources. Do **not** use an AWS account from the company you work for. __We strongly recommend that you use a non-production AWS account for this workshop such as a training, sandbox or personal account. If multiple participants are sharing a single account you should use unique names for the stack set and resources created in the console.__

---

To setup the workshop environment, launch the CloudFormation stack below in the preferred AWS region using the "Deploy to AWS" links below. This will automatically take you to the console to run the template.

**US West 2 (Oregon)** &nbsp; &nbsp; &nbsp; &nbsp; 
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-host.yml" target="_blank">![Deploy in us-est-2](/images/deploy-to-aws.png)</a>

---

**US East 2 (Ohio)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-host.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**US East 1 (N. Virginia)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-host.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**EU West 1 (Ireland)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-host.yml" target="_blank">![Deploy in us-east-1](/images/deploy-to-aws.png)</a>

---

**AP Southeast 2 (Sydney)** &nbsp; &nbsp; &nbsp; &nbsp;
<a href="https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=pww&templateURL=https://s3.amazonaws.com/protecting-workloads-workshop/public/artifacts/pww-workshop-env-build-host.yml" target="_blank">![Deploy in ap-southeast-2](/images/deploy-to-aws.png)</a>

---

1. Click **Next** on the Specify Template section.

2. On the Specify stack details step, update the following parameters depending on how you are doing this workshop:

- If you are sharing an AWS account with someone else in the same region, change the name of the stack to __pww-yourinitials__
- Automated Scanner: __Set to false.__
- Scanner Username: __Leave default.__
- Scanner Password: __Leave default.__
- Trusted Network CIDR: Enter a trusted IP or CIDR range you will access the site from using a web browser. You can obtain your current IP at <a href="https://ifconfig.co/" target="_blank">Ifconfig.co</a> The entry should follow CIDR notation. i.e. 10.10.10.10/32 for a single host.
- Keep the defaults for the rest of the parameters.


3. Click **Next**.

4. Click **Next** on the ***Configure stack options*** section.

5. Check the box to acknowledge that the template will create IAM roles under Capabilities and click **Create**.

This will bring you back to the CloudFormation console. You can refresh the page to see the stack starting to create. Before moving on, make sure the stack is in a __CREATE_COMPLETE__ status. This should take approximately eight minutes.

---

Click [here](./assess.md) to proceed to the Assess Phase.
