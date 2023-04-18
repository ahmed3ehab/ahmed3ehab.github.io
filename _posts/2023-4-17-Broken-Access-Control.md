---
layout: post
title: Broken Access Control
date: 2023-04-17
categories: [Cybersecurity, Web application penetration testing]
cover_image: /media/Broken-Access-Control.png
og_image: /media/Broken-Access-Control.png
---

- Broken access control refers to a type of security vulnerability where a user is able to access resources or perform actions that they should not be able to due to improperly implemented access controls. This type of vulnerability can occur in various types of software and systems, such as web applications, operating systems, and databases.

- Broken access control vulnerabilities can be mitigated through proper access control design and implementation, including the use of strong authentication mechanisms, role-based access control, and regular security testing and auditing. It is important for software developers and system administrators to stay up-to-date with the latest security best practices and patches to prevent these types of vulnerabilities from occurring.

## Access control vulnerabilities and privilege escalation

- `Access control vulnerabilities` refer to weaknesses in a system's security mechanisms that allow unauthorized access to resources, data, or functionality. `Privilege escalation`, on the other hand, is a type of attack that occurs when an attacker gains higher privileges than they are authorized to have, typically by exploiting a vulnerability in the system.

![](/images/access-control.svg)

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

For a practical lab visit :
```
https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality  
```

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

For a practical lab : 
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
