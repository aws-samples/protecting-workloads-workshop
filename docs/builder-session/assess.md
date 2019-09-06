# Supercharging your Workload Defenses - Assess Phase

!!! info "Important note!"
    __We are testing out some new ideas here.  Some things may not work as expected.  We appreciate your patience.__

You have either just deployed a CloudFormation stack or AWS has done so on your behalf at an event.
The CloudFormation stack contains
a PHP website on Amazon EC2 instances behind an application load balancer.
You are now going to assess the posture of the site at both the host layer and at the network layer.

In this section you will do the following tasks:

1. Examine the CloudFormation stack you built to learn about the tags that were applied by AWS CloudFormation.
2. Learn about Amazon Inspector targets, templates, and rules packages.
3. Examine Amazon Inspector findings
4. Assess the network layer by running an external network scan using a network scanner.

## Examine the stack that you built

1. At this point, you have either deployed a CloudFormation stack yourself or AWS has done so on your behalf.  You will need the name of this stack to proceed.  Go to the CloudFormation console.  You should see a list of stacks similar to the figure below.

    ![cloudformation-stack-list](./images/assess-cloudformation-stacks.png)

    If the console looks different than this, you may be using a newer version of the console.  In that case, use the menu on the left and click **Previous console**.

    If you built the stack yourself, you should see the name you supplied in the Stack Name column.  If AWS built the stack for you, the stack name will likely much longer name, for example it may start with "module" with a random string after that as in the figure above. Make sure you see the entire stack name.  If the stack name ends with three trailing periods ("..."), then widen the Stack Name column so you can see the entire name of the stack.  Copy the stack name into a scratch file on your workstation.

## Assess the Host Layer

### Setting up Amazon Inspector

Now that you know the name of your AWS CloudFormation stack, you will configure Amazon Inspector to scan the instance for vulnerabilities.  You will first define the *target* for inspector, namely the instances that Inspector will scan.  You will then define the *template* which includes both the *target* you defined as well as the kinds of scans to run.  You will then run the scan against the template.

### Configure the Amazon Inspector target

1. Go to the Amazon Inspector console and click **Get Started** if prompted.  If you see a "Welcome to Amazon Inspector" wizard appear, click **Cancel**.  You will set up Amazon Inspector manually so you can become more familiar with the service.

2. Click **Assessment Targets** on the left menu and then click the **Create** button.

3. Scroll down to the Assessment Target window.  In the *Name* field, enter a name of your choosing, such as **mytargets**.

4. In the *Use Tags* section, select **aws:cloudformation:stack-name** for the key from the drop down list and select the name of the stack from the drop down value list.  The reason you can do this is that CloudFormation adds a tag named aws:cloudformation:stack-name to every resource that it provisions.  When you configure Amazon Inspector, rather than specifying instance IDs, you will tell Inspector to look for instances with a specific tag.  Suppose that, in a production situation, you have to add more instances to handle a higher load.  Rather than modifying your Inspector target list, you can add the tag to the new instances and the target will then be able to select them automatically.

5. Make sure *Install Agents* check box is checked. This will cause Amazon Inspector to install the Inspector agent on the instances on your behalf.

6.  Click the **Save** button to save the target definition.  Inspector may prompt you for permission to create a service linked role to give the Amazon Inspector service permission to retrieve information about your instances.  If you see a prompt like the one in the figure below, click **Ok** to create the role.

    ![inspector-service-role](./images/assess-inspector-slr.png)

    You have now created an Amazon Inspector target that identifies the instances that would be assessed.  The target selects instances based on tag values.  Again, the tag you are using is *aws:cloudformation:stack-name* which is set to the name of the CloudFormation stack.

### Configure the Amazon Inspector template and run the assessment

Now that you have created an Amazon Inspector target, you will now create an Amazon Inspector template.  You use templates to define the Amazon Inspector targets and rule packages that comprise an assessment run.

1.  Go to the Amazon Inspector console, click **Assessment templates** on the menu, and then click **Create**.

2.  In the *Name* field, enter a name for the template, such as **mytemplate**.

3.  In the *Target name* field, select the target you previously created from the list of options.

4.  In the *Rules packages* field, select **Common Vulnerabilities and Exposures** and **Security Best Practices**.

5.   In the *Duration* field, select **15 minutes**.  Do **not** accept the default value!

6.  In the Assessment Schedule, uncheck (turn off) the *Set up recurring assessment* runs so that the assessment template will only run a one-time assessment.

7.  Scroll to the bottom and click the **Create and run** button to save the template and run the assessment.  Depending on the size of your screen, you may have to scroll down multiple windows.  If you cannot click **Create and run**, make sure you unchecked the box in the previous step and try again.  The assessment will start and will take 15 minutes to compete.

8.  On the Amazon Inspector menu, click **Assessment runs**.  You should see an entry for the assessment you just started.  While the assessment is running, the status will be *Collecting data*.  Periodically refresh the screen to see the current status.  When the assessment run ends, the status will change to *Analysis complete.*  The assessment will run for approximately 15 minutes. **_While you are waiting, continue with the steps below._**

## Assess the Network Layer

### Identify the Application Load Balancer and Connect to the RedTeam Host

1.  Go to the stack outputs and look for the website URL stored in the **albEndpoint** output value. Test access to the site by right clicking and opening in a new tab. Note the URL for your site as this will be used throughout this workshop round.

2. While still in stack outputs, right click the link in **RedTeamHostSession** and open in new tab. This will launch an AWS Systems Manager Session Manager to the host you will use to perform ad hoc scans against your site URL.

!!! info "AWS Systems Manager Session Manager"
    Session Manager is a fully managed AWS Systems Manager capability that lets you manage your Amazon EC2 instances through an interactive one-click browser-based shell or through the AWS CLI. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.

### Using the Scanner

In order to test your AWS WAF ruleset, this lab has been configured with two scanning capabilities; a Red Team Host where you can invoke manual scanning and an automated scanner which runs from outside your lab environment.

The scanner performs 10 basic tests designed to help simulate and mitigate common web attack vectors.

1. Canary GET - This should succeed and shows that the scanner is not being blocked.
2. Canary POST - This should succeed and shows that the scanner is not being blocked.
3. SQL Injection (SQLi) attack in Query String
4. SQL Injection (SQLi) attack in Cookie
5. Cross Site Scripting (XSS) attack in Query String
6. Cross Site Scripting (XSS) attack in Body

!!! info "Note about Tests"
    _These basic tests are designed to provide common examples you can use to test AWS WAF functionality. You should perform thorough analysis and testing when implementing rules into your production environments._

Once you have started a Session Manager connection to your Red Team Host, the scanner script can be invoked by typing the following command while in the _/usr/bin_ directory:

````
python3 scanner.py http://your-alb-endpoint
````
![Initial Scan Terminal](./images/initial-scan-term.svg)

The scanner.py script will run each of the tests above and report back the following information:

- __Request__: The HTTP request command used.
- __Test Name__: The name of the test from list above.
- __Result__: The HTTP status code returned.

The logic in the scanner script color codes the response as follows:

- __Green__: 403 - Forbidden (_Except for canary GET and POST tests._)
- __Red__: 200 - OK
- __Blue__: 404 - Not Found
- __Yellow__: 500 - Internal Server Error

As you can see by running the script there are several vulnerabilities that need to be addressed. In the remediate phase you will configure an AWS WAF Web ACL to block these requests. When AWS WAF blocks a web request based on the conditions that you specify, it returns HTTP status code 403 (Forbidden). For a full view of the request and response information, you can paste the **Request** command directly into the console and add the --debug argument.

!!! info "Note about Testing Tool"
    The scanner.py script uses an open source <a href="https://httpie.org/" target="_blank">HTTP client called httpie</a>. HTTPie—aitch-tee-tee-pie—is a command line HTTP client with an intuitive UI, JSON support, syntax highlighting, wget-like downloads, plugins, and more.

*This completes the Assess Phase.*

---

Click [here](remediate.md) to proceed to the Remediate Phase.
