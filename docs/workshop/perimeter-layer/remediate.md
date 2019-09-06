# Mitigating Common Web Application Attack Vectors Using AWS WAF - Remediate Phase

In the previous Build Phase, you identified several vulnerabilities in your web application.
You are now going to design and implement an AWS WAF ruleset to help mitigate these vulnerabilities. In this section you will do the following tasks:

1. Identify the WAF ACL for your site
2. AWS WAF Rule design and considerations
3. Console Walkthrough - Creating a WAF Condition and Rule
4. WAF Rule Creation and Solutions

## Identify the WAF ACL for your Site

1. If needed, go to <a href="https://console.aws.amazon.com/console/home" target="_blank">https://console.aws.amazon.com/console/home</a>. You will be redirected to the AWS Management Console dashboard on successful login:
![Console Home](./images/console-home.png)
Make sure you select the appropriate AWS Region when working in the AWS Management Console (top right corner, on the menu bar).

2. From the Management Console dashboard, navigate to the AWS WAF & Shield service console. You can do that several ways:
    - Type “waf” in the AWS services panel search box and select the resulting option
    - Expand the Services drop down menu (top left on the menu bar) and choose WAF & Shield
    - Expand the All services area of the AWS services panel and choose WAF & Shield
Once selected, you will be redirected to the AWS WAF & AWS Shield service console. You may see an initial landing page at first. Choose Go to AWS WAF:

![WAF Home](./images/waf-home.png)
3. In the side bar menu on the left, pick the Web ACLs option under the AWS WAF heading. If the list of Web ACLs appears empty select the correct AWS Region as indicated on your credentials card in the Filter dropdown. If you are sharing the same account with other participants you can identify your WAF ACL by the Id in the stack outputs.

![WAF ACL Home](./images/waf-acl-home.png)
4. Click on the WAF Web ACL Name to select the existing Web ACL. Once the detail pane is loaded on the left of your screen, you will see 2 tabs: Requests and Rules. Toggle to Rules:

![WAF ACL Rules](./images/waf-acl-rules.png)
Validate that you are able to see a pre-existing rule, configured to block requests, and that your Web ACL is associated with an Application load balancer resource. You can drill down further into the properties of the existing rule, by clicking on the rule name. You should see 2 entries into the associated IP address list for the loopback/localhost IP addresses (127.0.0.0/8, ::1/128).

##AWS WAF Rule Design and Considerations

###Basics

AWS WAF rules consist of conditions. Conditions are lists of specific filters (patterns) that are being matched against the HTTP request components processed by AWS WAF. The filters, including their attributes, are specific to the type of condition supported by AWS WAF. A condition, as a whole, is considered as _matched_, if any one of the listed filters is matched.

Rules contain one or more conditions. Each condition attached to a rule is called a predicate. Predicates are evaluated using Boolean logic. A predicate is evaluated as matched or not matched (negated predicted), and multiple predicates are evaluated using Boolean AND – all predicates must match for the rule action to be triggered.

Web ACLs are ordered lists of rules. They are evaluated in order for each HTTP request and the action of the first matching rule is taken by the WAF engine, whether that is to allow, block or count the request. If no rule matches, the default action of the web ACL prevails.

![How AWS WAF Works](./images/how-waf-works.png)

!!! info "Note About Conditions and Rules"
    Conditions and rules are reusable resources within the region in which they are created.  You should consider the effects of changes to WAF conditions and rules in your organizations change control procedures.

!!! info "Note About This Section"
    **In order to illustrate the process of creating WAF conditions and rules, we will walk through the creation of the first rule in your WAF ACL.** The complete list of threats and solutions is available in the <a href="./#waf-rule-creation-and-solutions">WAF Rule Creation and Solutions</a> section.

###Rule Design Considerations:

To create a rule, you have to create the relevant match conditions first. This process requires planning for effective rule building. Use the following guiding questions:

1.	What is the intended purpose of the rule?
2.	What HTTP request components apply to the purpose of the rule?
3.	Do you already have conditions targeting those request components that you can reuse? Is that desirable?
4.	How can you define the purpose of the rule in a Boolean logic expression?
5.	What conditions do you need to create to implement the logic?
6.	Are any transformations relevant to my input content type?

####AWS WAF Concepts:

The following illustration shows AWS WAF Conditions, Rules and Web ACL's.

![AWS WAF Concepts](./images/waf-concepts.png)

The following illustration shows how AWS WAF checks the rules and performs the actions based on those rules.

![AWS WAF Concepts](./images/web-acl-3a.png)

###Example Rule Design and Creation:

As an example, lets say we want to build a rule to detect and block SQL Injection in received in query strings. Let’s see how these questions help us plan the implementation of the rule. _This walkthrough will get you started with the ruleset required to mitigate the simulated threats in the workshop. It's purpose is to help you better understand the rule creation process. You will create the remaining rules from solution hints provided below._

####Sample Rule purpose:

- **Detect SQL Injection in query string, use ‘block’ action in Web ACL**

####HTTP request components:

- **Request Method** – form input typically gets submitted using a GET HTTP request method
- **Query String** – the SQL injection attempt is located in the query string 

####Define the purpose of the rule using Boolean logic:

- If **Query String contains suspected SQL Injection** then **block**

####Sample Rule - Conditions to implement:

- **SQL injection Match Condition** targeting the request **Query string**

####Relevant transformations:

- **SQL Injection Match Condition** query string is URL encoded, so we will apply the **URL_DECODE** transformation.

####Rules to implement:

- Rule with 1 predicate matching SQL injection condition

##Console Walkthrough - Creating a Condition and Rule

1. In the AWS WAF console, create a SQL injection condition by selecting **SQL injection** matching from the side-bar menu to the left of the console, under the **Conditions** heading.

2.	Click on **Create Condition**:

![WAF Condition Home](./images/waf-condition-home.png)
3.	Provide **filterSQLi** for the **Name** and select the region where you deployed the stack. Add a filter (pattern) to the condition. Set the **Part of the request to filter on** to **Query string** and set the **Transformation** to **URL decode**. Click **Add filter** and then click **Create**.

![Create String Match](./images/create-sqli-match.png)
4. With the condition created, and any additional conditions created based on need as well, you are ready to create a rule. In the AWS WAF console, select **Rules** from the side-bar menu to the left of the console, under the **AWS WAF** heading.

5\.	Click on **Create Rule**:

![Create Rule](./images/waf-rules-home.png)
6.	Provide **matchSQLi** for the name, metric name and sect the region where you deployed the stack. Set the **rule type** to **Regular rule**.

![Create Rule Detail](./images/create-rule-detail.png)
7.	Add a condition to the rule. For our rule example, choose “When a request” **does** (no negation) **match at least one of the filters in the SQL injection match condition**. Choose the SQL injection condition you have previously created.

![Add Conditions](./images/add-conditions.png)
8.	Click **Add Condition** and click **Create** at the bottom of the screen.

9\. Follow the steps in the <a href="./#identify-the-waf-acl-for-your-site">Identify the WAF ACL for your site</a> section above to go back to the Rules tab of your web ACL.

10\.	Click **Edit web ACL**.
![Edit Web ACL](./images/edit-web-acl.png)

11\. In the **Rules** dropdown, select your rule, and click **Add rule to web ACL**.

12\. Reorder the rules as appropriate for your use case.

13\. Click **Update** to persist the changes.

!!! info "Additional Resources"
    For a more comprehensive discussion of common vulnerabilities for web applications, as well as how to mitigate them using AWS WAF, and other AWS services, please refer to the <a href="https://d0.awsstatic.com/whitepapers/Security/aws-waf-owasp.pdf" target="_blank">Use AWS WAF to Mitigate OWASP’s Top 10 Web Application Vulnerabilities whitepaper</a>.

## WAF Rule Creation and Solutions

In this phase, we will have a set of 6 exercises walking you through the process of building a basic mitigation rule set for common vulnerabilities. We will build these rules from scratch, so you can gain familiarity with the AWS WAF programming model and you can then write rules specific to your applications. 

!!! info "Note About Exercise Solutions"
    For the exercises below, you will find the high level description and solution configuration for your web ACL. You can test your ACL ruleset at any time using the Red Team Host. For AWS sponsored event, you can also view test results on the <a href="http://waflabdash.awssecworkshops.com/" target="_blank">WAF Lab Dashboard</a>.

### 1. SQL Injection & Cross Site Scripting Mitigation

Use the SQL injection, cross-site scripting, as well as string and regex matching conditions to build rules that mitigate injection attacks and cross site scripting attacks.

Consider the following:
- How does your web application accept end-user input (whether directly or indirectly). Which HTTP request components does that input get inserted into?
- What kind of content encoding considerations do you need to factor in for the input format?
- What considerations do you need to account for in regards to false positives? For example, does your application legitimately need to accept SQL statements as input?

How do the requirements derived from the above questions affect your solution?

??? info "Solution"
    1.	update the **SQL injection** condition named filterSQLi with 2 additional filters
        1. query_string, url decode _You should have created this filter in <a href="./#console-walkthrough-creating-a-condition-and-rule">the walk through above</a>_
        2. body, html decode
        3. header, cookie, url decode
    2.  create SQLi rule named matchSQLi
    	1. type regular
        2. does match SQLi condition: filterSQLi
    3.	create **Cross-site scripting** condition named filterXSS with 4 filters
        1. query_string, url decode
        2. body, html decode
        3. body, url decode
        4. header, cookie, url decode
    4.	create a **String and regex matching** _String match_ condition named filterXSSPathException with 1 filter. _This demonstrates how to add an exception for the XSS rule_ 
	    1. uri, starts with, no transform, _/reportBuilder/Editor.aspx_
    5.	create a rule named matchXSS
        1. type regular
        2. does match XSS condition: filterXSS
        3. does not match string match condition: filterXSSPathException
    6.	add rules to Web ACL
    7.  Re-run the WAF test script (scanner.py) from your red team host to confirm requests are blocked

### 2. Enforce Request Hygiene

Use the string and regex matching, size constraints and IP address match conditions to build rules that block non-conforming or low value HTTP requests.

Consider the following:
•	Are there limits to the size of the various HTTP request components relevant to your web application? For example, does your application ever use URIs that are longer than 100 characters in size?
•	Are there specific HTTP request components without which your application cannot operate effectively (e.g. CSRF token header, authorization header, referrer header)?

Build rules that ensure the requests your application ends up processing are valid, conforming and valuable.

??? info "Solution"
    1.	create **String and regex matching** _String match_ type condition named filterFormProcessor with 1 filter
        1.	uri, starts with, no transform, _/form.php_
    2.	create string match condition named filterPOSTMethod with 1 filter
        1.	uri, exactly matches, no transform, _/form.php_
    3.	create **String and regex matching** _Regex match_ condition named filterCSRFToken with 1 filter
        1.	header x-csrf-token (_type in manually_), url decode, matches pattern: _^[0-9a-f]{40}$_
    4.	create rule named matchCSRF
        1.	type regular
        2.	does match string condition: filterFormProcessor
        3.	does match string condition: filterPOSTMethod
        4.	does not match regex match condition: filterCSRFToken
    5.	add rules to Web ACL
    6.  Re-run the WAF test script (scanner.py) from your red team host to confirm requests are blocked

### 3. Mitigate File Inclusion & Path Traversal

Use the string and regex matching conditions to build rules that block specific patterns indicative of unwanted path traversal or file inclusion.

Consider the following:

- Can end users browse the directory structure of your web folders? Do you have directory indexes enabled?
- Is your application (or any dependency components) use input parameters in filesystem or remote URL references? 
- Do you adequately lock down access so input paths cannot be manipulated?
- What considerations do you need to account for in regards to false positives (directory traversal signature patterns)?  

Build rules that ensure the relevant HTTP request components used for input into paths do not contain known path traversal patterns.

??? info "Solution"
    1.	create a **String and regex matching** _String match_ type condition named filterTraversal with 3 filters
        1. uri, starts with, url_decode, _/include_
        2. query_string, contains, url_decode, _../_
        3. query_string, contains, url_decode, _://_
    2.	create rule named matchTraversal
        1. type regular
        2. does match string condition: filterTraversal
    3.	add rules to Web ACL
    4.  Re-run the WAF test script (scanner.py) from your red team host to confirm requests are blocked

!!! info "Note About Remaining Exercises"
    **The remaining exercises below are optional. You should proceed to the [Verify Phase](verify.md) and come back to the content below if time permits.**

---

### 4. Limit Attack Footprint (Optional)

Use the string and regex matching conditions along with geo match and IP address match conditions to build rules that limit the attack footprint against the exposed components of your application.

Consider the following:
•	Does your web application have server-side include components in the public web path?
•	Does your web application have components at exposed paths that are not used (or dependencies have such functions)?
•	Do you have administrative, management, status or health check paths and components that aren’t meant for end user access?

You should consider blocking access to such elements, or limiting access to known sources, either whitelisted IP addresses or geographic locations.

??? info "Solution"
    1.	create **Geo match** condition named filterAffiliates with 1 filter
        1.	add country US, and RO
    2.	create **String and regex matching** _String match_ type condition named filterAdminUI with 1 filter
        1.	uri, starts with, no transform, _/admin_
    3.	create rule named matchAdminNotAffiliate
        1.	type regular
        2.	does match string condition: filterAdminUI
        3.	does not match geo condition: filterAffiliates
    4.	add rule to Web ACL

### 5. Detect & Mitigate Anomalies (Optional)

What constitutes an anomaly in regards to your web application? A few common anomaly patterns are:

- Unusually elevated volume of requests in general
- Unusually elevated volumes of requests to specific URI paths
- Unusually elevated levels of requests generating specific non-HTTP status 200 responses
- Unusually elevated volumes from certain sources (IPs, geographies)
- Usual request signatures (referrers, user agent strings, content types, etc)

Do you have mechanisms in place to detect such patterns? If so, can you build rules to mitigate them?

??? info "Solution"
    1.	create **String and regex match** condition named filterLoginProcessor with 1 filter
        1.	uri, starts with, no transform, _/login.php_
    2.	create rule named matchRateLogin
        1.	type rate-based, 2000
        2.	does match string condition: filterLoginProcessor
        3.	does match string condition: filterPOSTMethod
    3.	add rules to Web ACL

### 6. Reputation Lists, Nuisance Requests (Optional)

Reputation lists (whitelists or blacklists) are a good way to filter and stop servicing low value requests. This can reduce operating costs, and reduce exposure to attack vectors. Reputation lists can be self-maintained: lists of identifiable actors that you have determined are undesired. They can be identified any number of ways:

- the source IP address
- the user agent string
- reuse of hijacked authorization or session tokens,
- attempting to make requests to paths that clearly do not exist in your application but are well known vulnerable software packages (probing)

Build blacklists of such actors using the relevant conditions and set up rules to match and block them. An example IP-based blacklist already exists in your sandbox environment.

Reputation lists can also be maintained by third parties. The AWS WAF Security Automations allow you to implement IP-based reputation lists.

??? info "Solution"
    1.	edit the IP addresses condition named WafIpBlackList
        1. add a test IP address _You can obtain your current IP at <a href="https://ifconfig.co/" target="_blank">Ifconfig.co</a> The entry should follow CIDR notation. i.e. 10.10.10.10/32 for a single host._
    2.	create a **String and regex matching** _String match_ condition named filterNoPath with 1 filter
        1.	uri, starts with, no transform, _/phpmyadmin_
    3.	Use the concepts you learned in the previous exercises to add the _filterNoPath_ condition to your Web ACL.

---

You can now proceed to the [Verify Phase](verify.md).
