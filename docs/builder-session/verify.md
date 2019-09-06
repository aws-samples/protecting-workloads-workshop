# Supercharging your Workload Defenses - Verify Phase

## Host Layer - Verify Phase

Now that you have remediated the environment, you will again use Amazon Inspector to assess the environment again to see how the patching affected the overall security posture of the environment.

1.  Make sure the patches have completed installing. Go to the Amazon Inspector console, click **Assessment templates** on the menu.

2.  Locate the template that you created during the Assess Phase and confirm the run has completed.

3.  If needed, click **Assessment runs** and periodically refresh the screen.  Wait until the status for the run changes to *Analysis complete*.  The run will take approximately 15 minutes to complete.

4.  Compare the number of findings between the two runs.   In most cases, there will be fewer findings in the newer run since patches have been applied.   The change in the number findings may vary based on the age of the AMI used to launch the instances.

5.  Click the number of findings for the newest run (after the patches were installed).  You will then see all of the findings that were not patched during the Remediate Phase.

6.  Take a look at the entries that were not patched.  A common example of a finding is an instance is configured to allow users to log in with root credentials over SSH, without having to use a command authenticated by a public key.  Why would Patch Manager not patch this or the other findings?

7.  You have now completed this round.  Click [here](./cleanup.md) to proceed to the Cleanup Phase.

## Perimeter Layer - Verify Phase

In the previous remediation phase, you implemented an AWS WAF ruleset to protect your site from common attack vectors. You are now going to reassess the posture of the site to confirm the rules are performing as intended and blocking the simulated malicious requests.

1. Confirm malicious requests are blocked by WAF policy
2. Implement WAF monitoring dashboard using Amazon CloudWatch (Optional)

## Confirm malicious requests are blocked by WAF policy

If needed, start a Session Manager connection to your Red Team Host, the scanner script can be invoked by typing the following command while in the _/usr/bin_ directory:

````
python3 scanner.py http://your-alb-endpoint
````

Confirm that all of the tests in the script pass. If requests (other than canary) are not being blocked, go back to the remediate phase and confirm your conditions and rules are properly configured.

![Final Scan Terminal](./images/final-scan-term.svg)


__Congratulations! You have completed the Builder Session and learned how to supercharge your defenses at the network and host layers!__

---

!!! info "Additional Resources"
    For a more comprehensive discussion of common vulnerabilities for web applications, as well as how to mitigate them using AWS WAF, and other AWS services, please refer to the <a href="https://d0.awsstatic.com/whitepapers/Security/aws-waf-owasp.pdf" target="_blank">Use AWS WAF to Mitigate OWASP’s Top 10 Web Application Vulnerabilities whitepaper</a>.

## Implement WAF monitoring dashboard using Amazon CloudWatch (Optional)

If you finish early, <a href="https://aws.amazon.com/blogs/aws/cloudwatch-dashboards-create-use-customized-metrics-views/" target="_blank">use CloudWatch Dashboards to create a monitoring system for your protection layer</a>.

Here are some sample of metrics that you can use. Starting from top left side, in clockwise order, we have:

![CloudWatch WAF Dasboard](./images/waf-cw-dash.png)

- **Allowed vs Blocked Requests**: if you receive a surge in allowed access (2 times normal peak access) or blocked access (any period that identifies more than 1,000 blocked requests), you can configure CloudWatch to send an alert. The idea here is to track known DDoS (when blocked requests increase) or new version of attack (when the requests are allowed to access the system);

- **BytesDownloaded vs. Uploaded**: help you identify when DDoS attack targets a service that doesn't need to receive a huge amount of access in order to exhaust resources (ex: search engine component sending MBs of information for one specific request parameters set);

- **ELB Spillover and Queue length**: use these metrics to verify if the attack is already causing damage to the infrastructure and/or for some reason, the attacker is bypassing protection layer and attacking directly unprotected resources;

- **ELB Request Count**: same as above, helps you identify damage by checking if the attacker is bypassing protection layer and/or CloudFront cache; review rules to increase cache hit rate;

- **ELB Healthy Host**: another system health check metric;

- **ASG CPU Utilization**: identify if the attacker is not only bypassing the CloudFront/WAF but also the ELB layer, also use to identify the damage impact of an attack;

### Learn more about Amazon Inspector Rules Packages

1.  Amazon Inspector offers a variety of rules packages that can be included in assessments.  The applicable rules packages may vary by operating system.   The Common Vulnerabilities and Exposures assessment is based on the CVE project that is hosted at [cve.mitrei.org](https://cve.mitre.org).  Open a new tab in your browser to [cve.mitre.orf](https://cve.mitre.org).  Click on **Search CVE List**.  Enter **CVE-2018-20169** into the search field and click **Submit**.  This shows you how to research known vulnerabilities.

2.  The Security Best Practices rule package examines some common configuration settings for some of the most common Amazon Linux settings. You can read more about this rule package [here](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_security-best-practices.html).

---

Click [here](cleanup.md) to proceed to the Cleanup Phase.
