---
layout: post
title: Broken Access Control
date: 2023-04-17
categories: [Cybersecurity, Web application penetration testing]
cover_image: /images/access-control.svg
og_image: /images/access-control.svg
---

## Broken Access Control

![](/images/access-control.svg)

Broken access control refers to a type of security vulnerability where a user is able to access resources or perform actions that they should not be able to due to improperly implemented access controls. This type of vulnerability can occur in various types of software and systems, such as web applications, operating systems, and databases.

## Access control vulnerabilities and privilege escalation

`Access control vulnerabilities` refer to weaknesses in a system's security mechanisms that allow unauthorized access to resources, data, or functionality. `Privilege escalation`, on the other hand, is a type of attack that occurs when an attacker gains higher privileges than they are authorized to have, typically by exploiting a vulnerability in the system.

---

## Access control 

- Access control (or authorization) is the application  of constraints on who (or what) can perform attempted actions or access resources that they have requested. In the context of web applications, access control is dependent on authentication and session management: 
     1.  `Authentication` identifies the user and confirms that they are who they say they are. 
     2.  `Session management` identifies which subsequent HTTP requests are being made by that same user. 
     3.  `Access control` determines whether the user is allowed to carry out the action that they are attempting to perform. 

- From a user perspective, access controls can be divided into the following categories: 
    1. `Vertical access controls`
    2. `Horizontal access controls`
    3. `Context-dependent access controls`

--- 

### Vertical access controls

 Vertical access controls refer to a type of access control mechanism that restricts access to resources based on an individual's hierarchical position or role within an organization. In other words, users are granted access to certain resources based on their level of authority or responsibility within the organization.

 Vertical access controls can be implemented through various means, including physical access controls such as keycard systems, or through `software-based access controls such as user permissions` and `role-based access control (RBAC)` systems. RBAC is a popular access control model that defines a set of roles within an organization, and assigns permissions to those roles based on the level of access required for each role.

 For example, in a company with a hierarchical structure, the CEO might have access to all resources within the organization, while lower-level employees might only have access to resources that are relevant to their specific job duties. Vertical access controls ensure that sensitive information and resources are only accessible to those with a need-to-know, and can help prevent data breaches and other security incidents.


### Horizontal access controls
 Horizontal access controls are a type of access control mechanism that provides access to resources based on a user's role, permissions, or attributes, rather than based on the user's identity. These controls are typically applied to a group of users who have the same level of access to a resource or set of resources

 Horizontal access controls are commonly used in large organizations where many people have access to the same resources. By assigning roles and permissions to users based on their job function or other relevant attributes, organizations can ensure that employees only have access to the resources they need to do their jobs, while minimizing the risk of unauthorized access.

 Examples of horizontal access controls include `role-based access control (RBAC)`, `attribute-based access control (ABAC)`, and `policy-based access control (PBAC)`. These access control models allow administrators to set up policies that dictate who has access to what resources based on a set of defined rules. For example, an RBAC policy might dictate that all managers have access to financial reports, while an ABAC policy might restrict access to a certain database to users who have a specific security clearance.

 Overall, horizontal access controls are an important part of an organization's security infrastructure, as they can help reduce the risk of data breaches and other security incidents by ensuring that only authorized users have access to sensitive resources.


### Context-dependent access controls

 Context-dependent access controls refer to a type of access control mechanism that takes into account various contextual factors when granting access to resources or performing actions. This type of access control allows for more granular and flexible permissions management, which can be especially useful in complex or dynamic environments.

 Contextual factors that can be considered in context-dependent access controls include:
1. `Time of day`: Access to certain resources or actions may be restricted to specific times of day, such as during business hours only.
2. `Location`: Access to resources or actions may be restricted based on the physical location of the user, such as allowing access only from a specific network or geographic location.
3. `User behavior`: Access to resources or actions may be restricted based on the behavior of the user, such as detecting unusual or suspicious activity and limiting access until further authentication or investigation.
4. `Device security`: Access to resources or actions may be restricted based on the security posture of the device being used, such as requiring certain security features like encryption or two-factor authentication.

Context-dependent access controls can be implemented through various means, including user and device profiling, network segmentation, and policy-based controls. This type of access control can help organizations to better manage and secure their resources by taking into account the dynamic and evolving nature of modern IT environments.

--- 

## Examples of broken access controls

---

## Vertical privilege escalation

 This occurs when a user is able to access resources or perform actions that should only be available to users with higher privileges or authority within the organization For example, if a non-administrative user can in fact gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation. 
 
---

### Unprotected functionality

Unprotected functionality is a security vulnerability that occurs when a software application exposes certain functionalities or features to users without proper authentication or authorization controls. This means that users are able to access and use features that they should not have access to, which can result in unauthorized actions, data breaches, or other security incidents.

Some examples of unprotected functionality vulnerabilities include:
1. `Accessing administrative features`: An attacker may be able to access administrative features of an application without proper authentication or authorization controls, allowing them to modify settings, access sensitive information, or perform other unauthorized actions.
2. `Accessing user data`: An attacker may be able to access user data, such as personal information or login credentials, without proper authentication or authorization controls.
3. `Accessing premium features`: An attacker may be able to access premium features or services of an application without proper authentication or authorization controls, allowing them to use these features without paying or subscribing.
4. `Bypassing security measures`: An attacker may be able to bypass security measures, such as CAPTCHA or two-factor authentication, without proper authentication or authorization controls, allowing them to access or modify information or functionality they should not have access to.

For example, a website might host sensitive functionality at the following URLs:

```
https://insecure-website.com/admin
https://insecure-website.com/robots.txt
```
Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive functionality.

In some cases, sensitive functionality is not robustly protected but is concealed by giving it a less predictable URL: so called security by obscurity. Merely hiding sensitive functionality does not provide effective access control since users might still discover the obfuscated URL in various ways.

For example, consider an application that hosts administrative functions at the following URL:

```
https://insecure-website.com/administrator-panel-yb556
``` 

This might not be directly guessable by an attacker. However, the application might still leak the URL to users. For example, the URL might be disclosed in JavaScript that constructs the user interface based on the user's role:

```js
    <script>
        var isAdmin = false;
        if (isAdmin) {
	        ...
	        var adminPanelTag = document.createElement('a');
	        adminPanelTag.setAttribute('https://insecure-website.com/administrator-panel-yb556'); // here is the admin panal
	        adminPanelTag.innerText = 'Admin panel';
	        ...
        }
    </script>
```

This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users regardless of their role.

For a practical lab:
```
https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality  

https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url
```

Unprotected functionality vulnerabilities can be mitigated through proper authentication and authorization controls, such as enforcing strong passwords, implementing multi-factor authentication, and limiting access to sensitive functionality or information. It is important for software developers and system administrators to regularly test and audit their applications to identify and remediate unprotected functionality vulnerabilities before they can be exploited by attackers

---

### Parameter-based access control methods

Parameter-based access control is a method of controlling access to a resource based on the values of parameters passed in the request. This method is commonly used in web applications, where users send requests to a server to perform various actions or retrieve information.

In parameter-based access control, the server checks the parameters in the request against a set of rules to determine whether the user is authorized to access the requested resource. For example, a user may send a request to view a specific page on a website, and the server may check whether the user is logged in and has the appropriate role or permissions to view that page.

For example:
```
https://insecure-website.com/login/home.jsp?admin=true
https://insecure-website.com/login/home.jsp?role=1
```
This approach is fundamentally insecure because a user can simply modify the value and gain access to functionality to which they are not authorized, such as administrative functions.

Some common parameter-based access control methods include:
1. `Role-based access control (RBAC)`: In RBAC, users are assigned roles that define their permissions and access to resources. Access control rules are defined based on the user's role rather than their individual identity.
2. `Attribute-based access control (ABAC)`: In ABAC, access control rules are based on attributes associated with the user, the resource, or the environment in which the request is made. For example, access may be granted based on the user's location, time of day, or other contextual factors.
3. `Mandatory access control (MAC)`: In MAC, access control rules are defined by a central authority based on the security classification of the resource and the user's security clearance level.
4. `Discretionary access control (DAC)`: In DAC, access control rules are defined by the owner of the resource, who determines which users or groups have access to the resource.

Parameter-based access control can be an effective method for controlling access to resources in web applications. However, it is important to ensure that access control rules are properly defined and enforced to prevent unauthorized access. Regular testing and auditing can help identify and remediate any vulnerabilities in the access control system.

For a practical lab: 
```
https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter

https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile
```

---

### Broken access control resulting from platform misconfiguration

Broken access control resulting from platform misconfiguration is a common type of security vulnerability that occurs when a software platform, such as a web application or database, is configured incorrectly or with default settings that leave it open to exploitation by attackers. This can result in unauthorized access to sensitive data, modification or deletion of data, and other security incidents.

Some applications enforce access controls at the platform layer by restricting access to specific URLs and HTTP methods based on the user's role. For example an application might configure rules like the following:
```
DENY: POST, /admin/deleteUser, managers
```
This rule denies access to the `POST` method on the URL `/admin/deleteUser`, for users in the managers group. Various things can go wrong in this situation, leading to access control bypasses.

Some examples of platform misconfigurations that can lead to broken access control include:

1. `Default passwords`: A platform is shipped with default passwords that are not changed, or are easy to guess, allowing attackers to gain access to the platform.
2. `Misconfigured access controls`: Access controls, such as file permissions or user roles, are misconfigured, allowing unauthorized access to sensitive data or functionality.
3. `Insecure protocols`: Insecure protocols, such as HTTP instead of HTTPS, are used to communicate with the platform, allowing attackers to intercept sensitive data in transit.
4. `Misconfigured network settings`: Network settings, such as firewalls or router configurations, are misconfigured, allowing attackers to gain unauthorized access to the platform from the network.

Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as `X-Original-URL` and `X-Rewrite-URL`. If a web site uses rigorous front-end controls to restrict access based on URL, but the application allows the URL to be overridden via a request header, then it might be possible to bypass the access controls using a request like the following:
```
POST / HTTP/1.1
X-Original-URL: /admin/deleteUser
....
```

An alternative attack can arise in relation to the HTTP method used in the request. The front-end controls above restrict access based on the URL and HTTP method. Some web sites are tolerant of alternate HTTP request methods when performing an action. If an attacker can use the `GET` (or another) method to perform actions on a restricted URL, then they can circumvent the access control that is implemented at the platform layer.

For a practical lab : 
```
https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented
```

---

### Broken access control resulting from URL-matching discrepancies

URL-matching discrepancies occur when the access control mechanism in a web application uses URL patterns to determine which users are allowed to access which resources. If the URL patterns are not properly defined or implemented, it can result in a mismatch between what the user is allowed to access and what they actually can access.

- Let's say we have a web application that allows users to view and edit their own profile information. The application uses a URL pattern of `"/users/{userId}"` to allow users to access their profile page. The application also has an access control mechanism that verifies whether the user is logged in and whether the requested profile belongs to the logged-in user.

    However, due to a programming error, the application is not properly validating the "userId" parameter in the URL. As a result, an attacker can manipulate the URL and access other users' profiles by changing the "userId" parameter.

    For example, if the attacker changes the URL from `"/users/123"` (where "123" is the attacker's own user ID) to `"/users/456"` (where "456" is the ID of another user), the application will allow the attacker to access the profile information of the other user, even though they are not authorized to do so.

- Another example, consider a web application that has a URL pattern of `"/admin/*"` to indicate that any URL starting with `"/admin/"` requires administrator-level access. However, if the application does not properly validate this pattern, a user may be able to access an administrative function by simply changing the URL from `"/user/profile"` to `"/admin/profile"`.

- Another example, they may be tolerant of inconsistent capitalization, so a request to `/ADMIN/DELETEUSER` may still be mapped to the same `/admin/deleteUser` endpoint. This isn't an issue in itself, but if the access control mechanism is less tolerant, it may treat these as two distinct endpoints and fail to enforce the appropriate restrictions as a result.

- Similar discrepancies can arise if developers using the Spring framework have enabled the `useSuffixPatternMatch` option. This allows paths with an arbitrary file extension to be mapped to an equivalent endpoint with no file extension. In other words, a request to `/admin/deleteUser.anything` would still match the `/admin/deleteUser` pattern. Prior to Spring 5.3, this option is enabled by default.

- On other systems, you may encounter discrepancies in whether `/admin/deleteUser` and `/admin/deleteUser/` are treated as a distinct endpoints. In this case, you may be able to bypass access controls simply by appending a trailing slash to the path. 

--- 

## Horizontal privilege escalation

Horizontal privilege escalation is a type of security vulnerability that occurs when a user gains access to resources or performs actions that they are not authorized to perform, but within the same level of privilege. In other words, the user is able to access resources or perform actions that are intended for another user with the same level of access.

For example, let's say there are two users, User A and User B, who both have the same level of access in a web application. User A is able to view and modify their own profile, but should not be able to view or modify User B's profile. However, due to a flaw in the application's access control mechanism, User A is able to access and modify User B's profile as well.

This type of vulnerability can be exploited by attackers to gain access to sensitive data or perform unauthorized actions. For example, an attacker who gains access to another user's profile may be able to view their personal information or modify their account settings.

Horizontal privilege escalation attacks may use similar types of exploit methods to vertical privilege escalation. For example, a user might ordinarily access their own account page using a URL like the following: 
```
https://insecure-website.com/myaccount?id=123
```
Now, if an attacker modifies the `id` parameter value to that of another user, then the attacker might gain access to another user's account page, with associated data and functions. 

In some applications, the exploitable parameter does not have a predictable value. For example, instead of an incrementing number, an application might use globally unique identifiers (GUIDs) to identify users. Here, an attacker might be unable to guess or predict the identifier for another user. However, the GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews. 

In some cases, an application does detect when the user is not permitted to access the resource, and returns a redirect to the login page. However, the response containing the redirect might still include some sensitive data belonging to the targeted user, so the attack is still successful. 

For Practical Labs:
```
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids

https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect
```

---

## Horizontal to vertical privilege escalation

A horizontal privilege escalation attack can be turned into a vertical privilege escalation, by compromising a more privileged user. For example, a horizontal escalation might allow an attacker to reset or capture the password belonging to another user. If the attacker targets an administrative user and compromises their account, then they can gain administrative access and so perform vertical privilege escalation

For example, an attacker might be able to gain access to another user's account page using the parameter tampering technique already described for horizontal privilege escalation: 

```
https://insecure-website.com/myaccount?id=456
```

If the target user is an application administrator, then the attacker will gain access to an administrative account page. This page might disclose the administrator's password or provide a means of changing it, or might provide direct access to privileged functionality. 

For Practical Lab:
```
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure
```

---

## Insecure direct object references (IDOR)

Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten. However, it is just one example of many access control implementation mistakes that can lead to access controls being circumvented. IDOR vulnerabilities are most commonly associated with horizontal privilege escalation, but they can also arise in relation to vertical privilege escalation. 

For example, let's say a website has a page that displays a user's account information. The website may use the user's ID as a parameter in the URL to retrieve and display the user's account details. If the website does not properly validate the user's ID before retrieving and displaying the account details, an attacker could manipulate the ID parameter to view another user's account information.

This type of vulnerability can lead to sensitive data exposure and unauthorized access to resources. To prevent IDOR vulnerabilities, it is important to implement proper access controls and validate user input properly. One way to prevent IDOR is to use indirect object references, where a unique identifier is used instead of a direct reference to an object. Additionally, applications should use proper authorization checks to ensure that users only have access to resources that they are authorized to access.

### IDOR vulnerability with direct reference to database objects

An IDOR vulnerability with direct reference to database objects can occur when an application uses user input to directly access and manipulate database objects without proper validation and authorization checks. For example, if an application uses a user's ID to retrieve and display data from a database, an attacker could modify the ID parameter to access and view data that they are not authorized to view.

Consider a website that uses the following URL to access the customer account page, by retrieving information from the back-end database: 

```
https://insecure-website.com/customer_account?customer_number=132355
```

Here, the customer number is used directly as a record index in queries that are performed on the back-end database. If no other controls are in place, an attacker can simply modify the customer_number value, bypassing access controls to view the records of other customers. This is an example of an IDOR vulnerability leading to horizontal privilege escalation. 

An attacker might be able to perform horizontal and vertical privilege escalation by altering the user to one with additional privileges while bypassing access controls. Other possibilities include exploiting password leakage or modifying parameters once the attacker has landed in the user's accounts page, for example. 


### IDOR vulnerability with direct reference to static files

IDOR vulnerabilities often arise when sensitive resources are located in static files on the server-side filesystem. For example, a website might save chat message transcripts to disk using an incrementing filename, and allow users to retrieve these by visiting a URL like the following: 

```
https://insecure-website.com/static/12144.txt
```

In this situation, an attacker can simply modify the filename to retrieve a transcript created by another user and potentially obtain user credentials and other sensitive data. 

For Practical Lab:
```
https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

https://cybertalents.com/challenges/web/silly-doors
```

--- 

## Access control vulnerabilities in multi-step processes

Many web sites implement important functions over a series of steps. This is often done when a variety of inputs or options need to be captured, or when the user needs to review and confirm details before the action is performed. For example, administrative function to update user details might involve the following steps: 

1. `Load form containing details for a specific user.`
2. `Submit changes. `
3. `Review the changes and confirm. `

Sometimes, a web site will implement rigorous access controls over some of these steps, but ignore others. For example, suppose access controls are correctly applied to the first and second steps, but not to the third step. Effectively, the web site assumes that a user will only reach step 3 if they have already completed the first steps, which are properly controlled. Here, an attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters. 

Multi-step processes can be vulnerable to access control vulnerabilities if the access controls are not properly designed and implemented. Here are some examples of access control vulnerabilities that can occur in multi-step processes:

- `Insufficient authentication`: If the authentication mechanism used in the multi-step process is not strong enough, it can be compromised by attackers. For example, if the authentication only requires a username and password, an attacker could easily guess or brute-force the password and gain access to the next step of the process.
- `Lack of authorization`: Even if the authentication mechanism is strong, if the authorization checks are not in place, attackers may be able to access steps they are not authorized to access. This can occur if the authorization checks are not properly implemented, or if they are bypassed by attackers who exploit vulnerabilities in the system.
- `Poorly designed workflow`: A poorly designed workflow can also lead to access control vulnerabilities in multi-step processes. For example, if the workflow does not require proper segregation of duties, a single user may be able to carry out all the steps in the process, which could lead to fraud or misuse of resources.
- `Inadequate logging and monitoring`: Without proper logging and monitoring, it can be difficult to detect access control violations in multi-step processes. This can occur if the system does not log all user activities or if the logs are not properly monitored.

For Practical Lab:
```
https://portswigger.net/web-security/access-control/lab-multi-step-process-with-no-access-control-on-one-step
```

---

## Referer-based access control

Referer-based access control is a technique used to control access to a web resource based on the HTTP Referer header sent by the client browser. The Referer header contains the URL of the page that the user was on before accessing the current page. Referer-based access control can be used to restrict access to resources based on the referring page or domain.

For example, a website owner may want to restrict access to a particular resource (such as a file or page) only to users who have come from a specific page or domain. To do this, the website can check the Referer header sent by the client browser when a user tries to access the resource. If the Referer header matches the expected value, the user is granted access; otherwise, access is denied.

While Referer-based access control can be useful in certain situations, it has some limitations and vulnerabilities that should be considered:

- `The Referer header can be easily spoofed`: Attackers can manipulate the Referer header to make it appear as though the request is coming from an authorized page or domain, allowing them to bypass the access control mechanism.
- `Referer headers are not always sent`: Some browsers do not send Referer headers, or users can disable them in their browser settings. In these cases, the access control mechanism may not function as expected.
- `The Referer header can reveal sensitive information:` The Referer header can reveal the URL of the referring page, which may contain sensitive information. This information could be intercepted by attackers or stored in logs, potentially exposing it to unauthorized parties.
- `Referer headers can be manipulated by intermediaries`: Intermediaries such as proxies and firewalls may modify or strip the Referer header, which could cause the access control mechanism to fail or behave unexpectedly.

Overall, while Referer-based access control can be a useful technique in some cases, it should not be relied upon as the sole mechanism for access control. Other techniques, such as user authentication and authorization, should be used in conjunction with Referer-based access control to provide a more robust and secure access control mechanism.

For Practical Lab:
```
https://portswigger.net/web-security/access-control/lab-referer-based-access-control
```

---

## Location-based access control

Location-based access control is a technique used to control access to resources based on the physical location of the user. This technique is commonly used in situations where it is necessary to restrict access to resources based on geographic or environmental factors, such as when access is restricted to a particular building or area.

Location-based access control can be implemented in several ways, including:
- `IP-based geolocation`: This technique involves mapping the user's IP address to a physical location using a geolocation database. Access to resources can then be restricted based on the user's location.
- `GPS-based location`: This technique involves using the GPS capabilities of the user's device to determine their physical location. Access to resources can then be restricted based on their proximity to a particular location.
- `Bluetooth-based proximity`: This technique involves using Bluetooth signals to determine the user's proximity to a particular location. Access to resources can then be restricted based on their distance from the location.
- `Wi-Fi-based locatio`n: This technique involves using the Wi-Fi network to determine the user's physical location. Access to resources can then be restricted based on their location within the network.

While location-based access control can be an effective technique for controlling access to resources, it also has some limitations and vulnerabilities that should be considered:
- `Location spoofing`: Attackers can spoof their location using various techniques, such as VPNs or proxies, to bypass the access control mechanism.
- `Signal interference`: GPS and Bluetooth signals can be disrupted by environmental factors, such as buildings or other electronic devices, which can affect the accuracy of location-based access control.
- `Privacy concerns`: Collecting and using location data can raise privacy concerns, particularly if the data is not properly secured or handled.
- `Inconvenience`: Location-based access control can be inconvenient for users who need to access resources from different locations or who have devices that are not equipped with the necessary technology.

Overall, location-based access control can be a useful technique in certain situations, but it should be used in conjunction with other access control techniques, such as user authentication and authorization, to provide a more robust and secure access control mechanism.

---

## How to prevent access control vulnerabilities

Preventing access control vulnerabilities involves implementing a range of security measures to protect against unauthorized access to resources. Here are some best practices for preventing access control vulnerabilities:

- `Implement access control policies`: Develop and implement a clear and consistent access control policy that specifies who is authorized to access which resources and under what conditions.
- `Use strong authentication and authorization mechanisms`: Require strong passwords, implement two-factor authentication, and use role-based access control to ensure that only authorized users have access to sensitive resources.
- `Regularly review and update access controls`: Regularly review access control policies and permissions to ensure that they remain relevant and effective, and revoke access for users who no longer require it.
- `Use encryption`: Encrypt sensitive data at rest and in transit to prevent unauthorized access to data, even if access controls are breached.
- `Use audit logs`: Monitor access to resources using audit logs to detect and respond to unauthorized access attempts.
- `Test for vulnerabilities`: Regularly test for vulnerabilities in access controls, using techniques such as penetration testing, to identify and remediate any weaknesses in the system.
- `Educate users`: Provide regular training to users on the importance of access control and how to identify and report suspicious activity.
- `Never rely on obfuscation alone for access control. `
- ` Unless a resource is intended to be publicly accessible, deny access by default. `
- `Wherever possible, use a single application-wide mechanism for enforcing access controls. `
-  `At the code level, make it mandatory for developers to declare the access that is allowed for each resource, and deny access by default. `

By implementing these best practices, organizations can significantly reduce the risk of access control vulnerabilities and ensure that sensitive resources are protected from unauthorized access. It's also important to stay up to date with the latest security trends and technologies to stay ahead of evolving threats.

---

## References 

PortSwigger Content : [Access control vulnerabilities and privilege escalation](https://portswigger.net/web-security/access-control) 

PortSwigger Labs : [Access control vulnerabilities and privilege escalation Labs](https://portswigger.net/web-security/access-control)

HarabHackSploit (youtube channel) : [Broken Access Control](https://www.youtube.com/watch?v=8gLOhMrIpVo)

Rana Khalil (youtube channel) : [Broken Access Control (Short Version)](https://www.youtube.com/playlist?list=PLuyTk2_mYISJxFXJDdkDZjXD4K1yl3NFU)

Rana Khalil (youtube channel) : [Broken Access Control (Long Version)](https://www.youtube.com/playlist?list=PLuyTk2_mYISId4_l9YET7Gv29cHcNguq-)