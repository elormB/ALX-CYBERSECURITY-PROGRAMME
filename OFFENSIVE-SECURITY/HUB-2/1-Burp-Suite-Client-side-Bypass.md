# Client-Side Validation Bypass

## 1. Background

Client-side validation is a security mechanism implemented within a web browser to validate user input and enforce application rules before requests are submitted to the server. However, relying exclusively on client-side controls for security can expose web applications to manipulation because JavaScript code and browser-side variables can be modified by users.

This challenge demonstrates how client-side security restrictions can be bypassed using browser Developer Tools. It focuses on examining JavaScript variables, modifying client-side data, and interacting directly with application API endpoints to understand why critical security controls must also be enforced on the server side.

## 2. Objectives

- The objective of this challenge was to understand how client-side validation works and identify its security limitations.
- The exercise involved examining browser-side JavaScript, modifying account balance variables, and sending HTTP requests directly to application endpoints to evaluate whether purchase restrictions were properly enforced.

## 3. Scope and Limitations

### Scope:
The assessment focused on the simulated Tech Store application and included:

- Examining client-side JavaScript validation.
- Identifying browser-side account balance restrictions.
- Modifying JavaScript variables through Developer Tools.
- Inspecting application API endpoints.
- Sending HTTP POST requests using the Fetch API.
- Evaluating the effectiveness of server-side purchase validation.
  
### Limitations:
- Burp Suite unavailable: Burp Suite could not be used to intercept and modify HTTP requests as originally intended. Browser Developer Tools and the Fetch API were used as alternatives.
- Simulated environment: Testing was performed against a controlled training application and does not represent a real-world e-commerce platform.
- Client-side testing: The assessment focused primarily on manipulating browser-side variables and crafting HTTP requests. It did not involve a comprehensive examination of the application's backend.
- Limited verification: Modifying the displayed account balance does not necessarily change the actual balance stored on the server. A successful purchase bypass could not be confirmed without verifying the server response and resulting account state.
- Limited test coverage: Other security areas, such as authentication, authorization, session management, payment processing, and database security, were outside the scope of this exercise.
The findings should therefore be treated as observations from a limited client-side validation assessment rather than a complete security audit.

## 4. Legal and Ethical Disclaimer
- This assessment was conducted strictly within an authorized cybersecurity training environment for educational and research purposes. All testing activities were limited to the simulated web application provided for the challenge.

- The JavaScript manipulation and direct HTTP requests were performed to understand how client-side validation mechanisms can be bypassed and why server-side validation is essential for application security.

- No unauthorized systems, websites, accounts, or third-party services were accessed or tested. The techniques demonstrated in this exercise must only be applied to systems for which explicit permission has been granted.

- The information documented is intended to support ethical hacking education, security awareness, and the identification of potential application security weaknesses. Any unauthorized use of these techniques may violate applicable laws and regulations.

## 5. Lab Initialization and Access
To begin the practical, the lab was confirmed to be active and accessible:

1. The training-shell terminal was launched and Hub 2 was started.
2. The running command was executed to validate that the environment was live: running # expected output: `hub-2 running`.
3. The browser was opened and the interface was accessed via http://localhost
4. Navigation proceeded to the web security challenge section.
5. All the required tasks were initiated and completed


## 6. Challenge Overview

The challenge presented a simulated online technology store where a student account had an initial balance of **$0.00**. Several products were available for purchase, including a wireless mouse, webcam, mechanical keyboard, 4K monitor, and premium laptop.

The application implemented client-side restrictions intended to prevent purchases exceeding the available account balance.

The objective was to examine these restrictions and demonstrate how browser-side validation could be manipulated.

## 7. Tools Used

| Tool | Purpose |
|---|---|
| Browser Developer Tools | Inspect and modify client-side JavaScript execution. |
| JavaScript Console | Execute JavaScript commands and interact with application variables. |
| Fetch API | Send HTTP requests directly to application endpoints. |
| Web Browser | Access and interact with the simulated Tech Store application. |
| Burp Suite Community | Intended tool for intercepting and modifying HTTP requests; unavailable during the exercise. |

## 8. Practical Implementation

### 8.1 Burp Suite Configuration Attempt

Burp Suite Community was initially selected as the primary tool for intercepting and modifying HTTP requests. The intended procedure involved configuring the browser to use Burp's proxy listener, installing its CA certificate, and intercepting requests sent to the application's API endpoints.

However, Burp Suite was not functioning correctly in the provided lab environment as it was not able to intercept web traffic in the lab environment. Consequently, browser Developer Tools were used as an alternative approach.

### 8.2 Modifying Client-Side Balance

The following JavaScript command was executed in the browser console:

```javascript
window.userBalance = 999999;
console.log('Balance bypassed. current: $' + window.userBalance);
```

**Explanation:**

The command modified the browser-side `userBalance` variable to demonstrate how client-side application data could be manipulated. The console displayed the updated balance.

This demonstrated that JavaScript variables stored in the browser should not be trusted as authoritative financial or authorization data.

### 8.3 Sending a Direct Purchase Request

The Fetch API was used to submit a POST request to the application's purchase endpoint:

```javascript
fetch('/api/purchase', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    cartTotal: 50,
    userBalance: 9999,
    adminOverride: true,
    items: ["Wireless Mouse"],
    cartItems: [
      {id: 1, name: 'Wireless Mouse', price: 50, quantity: 1}
    ]
  })
})
.then(r => r.json())
.then(d => {
  console.log('Purchase Result:', d);
  alert('Purchase Complete! ' + d.message);
});
```

**Explanation:**

The request submitted purchase information directly to the `/api/purchase` endpoint using the HTTP POST method. A JSON payload containing the cart total, user balance, product details, and an additional override parameter was included.

The response was processed as JSON and displayed in the browser console.

This demonstrated how direct API interaction can be used to examine whether an application independently validates purchase information on the server.

**Note:** The request code alone does not establish that the purchase succeeded. The actual API response and resulting application state must be examined to confirm whether server-side validation was bypassed.

## 9. Key Findings

| Area | Observation | Security Implication |
|---|---|---|
| Client-side validation | Purchase restrictions were implemented in browser-side JavaScript. | Browser-side controls can be modified by users. |
| JavaScript variables | The account balance variable could be reassigned in the console. | Client-side financial values cannot be trusted. |
| API endpoint | The application exposed a purchase endpoint. | API requests require independent server-side validation. |
| Request manipulation | The Fetch API allowed direct submission of custom JSON data. | Applications must validate all submitted parameters. |
| Server-side validation | The actual enforcement of purchase restrictions requires verification. | Missing server-side checks could allow unauthorized transactions. |

## 10. Troubleshooting

During the lab, troubleshooting procedures were followed to resolve command execution issues and verify that the training environment was functioning correctly.

The following steps were used:

1. **Verify the active shell:** The current shell was checked using `echo $SHELL` to confirm that the expected command-line environment was active.

2. **Restart the lab environment:** If the shell was incorrect or required services were unavailable, the `start_script.sh` script was executed again to initialize the lab environment.

3. **Verify the lab status:** The `running` command was used to confirm that the lab environment had started successfully.

4. **Check command syntax:** Commands were reviewed for incorrect syntax, missing quotation marks, invalid parameters, and improperly formatted JSON.

5. **Verify the application endpoint:** The browser was used to confirm that the training application was accessible before sending requests to its API endpoints.

6. **Inspect browser console output:** JavaScript errors and Fetch API responses were examined through the browser Developer Tools to identify failed requests or unexpected application behaviour.

7. **Check HTTP responses:** The response status and returned JSON data were inspected to determine whether requests were accepted, rejected, or processed with errors.

8. **Validate request formatting:** The HTTP method, endpoint path, `Content-Type` header, and JSON request body were reviewed to ensure that the request was correctly structured.

9. **Use alternative testing methods:** Since Burp Suite could not be used successfully, browser Developer Tools and the Fetch API were used to perform the client-side manipulation and direct API testing.

These troubleshooting procedures helped maintain a functional lab environment, identify execution issues, and ensure that testing remained within the designated training environment.

## 11. Security Recommendations

- Implement all critical business rules on the server side.
- Never rely on browser-side account balances or price calculations.
- Retrieve account balances and product prices from trusted server-side records.
- Validate product IDs, quantities, and transaction totals before processing purchases.
- Enforce authentication and authorization on every sensitive API endpoint.
- Reject unauthorized or unexpected parameters, including client-supplied administrative overrides.
- Maintain transaction logs and monitor suspicious purchase requests.

## 12. Lessons Learned

The exercise demonstrated that client-side validation should be treated as a usability feature rather than a security boundary. Browser Developer Tools can be used to inspect and modify JavaScript variables, while the Fetch API allows direct interaction with application endpoints. It also highlighted the importance of server-side validation, particularly when handling sensitive operations such as financial transactions and purchases. Additionally, alternative browser-based testing techniques can provide useful insight when a dedicated interception proxy such as Burp Suite is unavailable.

## 13. Conclusion

The challenge provided practical exposure to client-side validation weaknesses and API request manipulation. Although Burp Suite could not be used successfully in the lab environment, browser Developer Tools and the Fetch API provided an alternative means of examining client-side behavior and interacting with the application's purchase endpoint. The exercise reinforced the importance of implementing security controls on the server side rather than relying on browser-side JavaScript. Overall, the assessment demonstrated how insufficient validation of client-submitted data can introduce business logic vulnerabilities and potentially expose web applications to unauthorized transactions.