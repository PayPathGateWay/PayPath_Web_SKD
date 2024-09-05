# **PayPath_Web_SDK**

The **PayPath Web SDK** is a library designed to make payment integration simple, secure, and scalable for web applications. It provides easy-to-use APIs for performing payment transactions, managing users, and interacting with the PayPath Payment Gateway. This SDK is suitable for merchants and developers who want to integrate payment processing features into their websites.

---

## **Table of Contents**
- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [API Reference](#api-reference)
  - [Initialization](#initialization)
  - [Create a Payment](#create-a-payment)
  - [Tokenization](#tokenization)
  - [User Management](#user-management)
  - [Transaction History](#transaction-history)
  - [Notifications](#notifications)
- [Error Handling](#error-handling)
- [Security Considerations](#security-considerations)
- [Testing](#testing)
- [Contribution](#contribution)
- [License](#license)

---

## **Overview**

The PayPath Web SDK allows you to easily integrate PayPath Payment Gateway functionality into your web application. It provides methods to:
- Create and process payments with credit cards, virtual bank issuers (VBA), and tokenized cards.
- Handle user management, including CRUD operations for merchants and payers.
- Manage notifications and send receipts.
- Access and manage transaction histories.

---

## **Features**

- **Payment Processing:** Interact with **Vuiral Bank Issuer (VBA)** for verifying and processing payments.
- **Tokenization:** Securely tokenize sensitive card details.
- **User Management:** CRUD operations for managing users (merchants and payers).
- **Notifications:** Send notifications via SMS and email.
- **Transaction Handling:** Query and manage transaction histories.
- **Security:** Implements PCI-DSS compliance, encryption, and secure communication standards.

---

## **Installation**

To install the **PayPath_Web_SDK**, you can use npm:

```bash
npm install paypath-web-sdk
```

Or use yarn:

```bash
yarn add paypath-web-sdk
```

---

## **Quick Start**

Here’s how you can quickly get started with PayPath_Web_SDK:

1. **Initialize the SDK:**

   ```javascript
   import PayPath from 'paypath-web-sdk';

   const paypath = new PayPath({
       apiKey: 'your-api-key',
       environment: 'production', // or 'sandbox'
   });
   ```

2. **Process a Payment:**

   ```javascript
   paypath.createPayment({
       payerId: 'payer123',
       merchantId: 'merchant456',
       amount: 100.00,
       currency: 'USD',
       cardDetails: {
           cardNumber: '4111111111111111',
           expirationDate: '12/25',
           cvv: '123'
       }
   }).then(response => {
       console.log('Payment successful:', response);
   }).catch(error => {
       console.error('Payment failed:', error);
   });
   ```

3. **Tokenize Card Information:**

   ```javascript
   paypath.tokenizeCard({
       cardNumber: '4111111111111111',
       expirationDate: '12/25',
       cvv: '123'
   }).then(token => {
       console.log('Token created:', token);
   }).catch(error => {
       console.error('Tokenization failed:', error);
   });
   ```

---

## **Configuration**

To configure the SDK, pass an options object to the SDK during initialization:

```javascript
const paypath = new PayPath({
    apiKey: 'your-api-key',  // Required: API Key from PayPath
    environment: 'sandbox',  // Optional: Set 'production' or 'sandbox'
    baseURL: 'https://api.paypath.com' // Optional: Override default API URL
});
```

| Parameter  | Type     | Description                                |
|------------|----------|--------------------------------------------|
| apiKey     | `string`  | Your PayPath API key (required).           |
| environment| `string`  | `sandbox` or `production` (default: `production`). |
| baseURL    | `string`  | API base URL, default is PayPath’s production endpoint. |

---

## **API Reference**

### **Initialization**

To initialize the SDK:

```javascript
const paypath = new PayPath({
    apiKey: 'your-api-key',
    environment: 'sandbox'
});
```

### **Create a Payment**

To create a new payment:

```javascript
paypath.createPayment({
    payerId: 'payer123',
    merchantId: 'merchant456',
    amount: 100.00,
    currency: 'USD',
    token: 'secure_token' // Optional: Can use tokenized card details
}).then(response => {
    console.log('Payment successful:', response);
}).catch(error => {
    console.error('Payment failed:', error);
});
```

### **Tokenization**

To tokenize card details:

```javascript
paypath.tokenizeCard({
    cardNumber: '4111111111111111',
    expirationDate: '12/25',
    cvv: '123'
}).then(token => {
    console.log('Token created:', token);
}).catch(error => {
    console.error('Tokenization failed:', error);
});
```

### **User Management**

#### **Create a Payer:**

```javascript
paypath.createPayer({
    email: 'payer@example.com',
    preferredPaymentMethodId: 'method789',
    phone: '+1234567890'
}).then(response => {
    console.log('Payer created:', response);
}).catch(error => {
    console.error('Error creating payer:', error);
});
```

#### **Update a Merchant:**

```javascript
paypath.updateMerchant({
    merchantId: 'merchant456',
    email: 'merchant@example.com',
    isVerified: true
}).then(response => {
    console.log('Merchant updated:', response);
}).catch(error => {
    console.error('Error updating merchant:', error);
});
```

### **Transaction History**

To retrieve a payer’s transaction history:

```javascript
paypath.getPayerTransactions('payer123').then(transactions => {
    console.log('Transactions:', transactions);
}).catch(error => {
    console.error('Error fetching transactions:', error);
});
```

---

## **Error Handling**

The SDK provides error handling for different types of failures:

```javascript
paypath.createPayment({
    payerId: 'payer123',
    amount: 100.00
}).then(response => {
    console.log('Payment successful');
}).catch(error => {
    if (error.response) {
        console.error('Server Error:', error.response.data);
    } else {
        console.error('Network Error:', error.message);
    }
});
```

### Common Error Types:

1. **API Errors:** Issues with the API request (e.g., invalid API key).
2. **Validation Errors:** Missing or invalid fields (e.g., missing amount).
3. **Network Errors:** Issues connecting to the API.

---

## **Security Considerations**

- **PCI-DSS Compliance:** Ensure your application complies with PCI-DSS standards when using the PayPath SDK. Sensitive information, such as card details, should never be logged or stored in an insecure way.
- **HTTPS:** Always use **HTTPS** for secure communication between your web app and PayPath’s API.
- **Tokenization:** Use tokenized card details for transactions instead of handling raw card data.

---

## **Testing**

You can run automated tests for PayPath_Web_SDK using a testing framework like Jest or Mocha:

1. **Install Jest:**
   ```bash
   npm install --save-dev jest
   ```

2. **Run tests:**
   ```bash
   npm test
   ```

3. **Writing Tests:**
   Sample unit test:
   ```javascript
   test('should create a payment', async () => {
       const payment = await paypath.createPayment({
           payerId: 'payer123',
           amount: 100.00
       });
       expect(payment.status).toBe('success');
   });
   ```

---

## **Contribution**

If you want to contribute to the PayPath SDK:

1. Fork the repository.
2. Make changes in your forked copy.
3. Submit a pull request with detailed notes on your changes.

---

## **License**

The PayPath_Web_SDK is open-sourced software licensed under the **MIT License**.

---

## **Additional Resources**

- [API Documentation](https://docs.paypath.com)
- [Official Website](https://www.paypath.com)
- [Support](mailto:support@paypath.com)

---

### **End of README**

This README.md offers a comprehensive guide to help developers get started with the **PayPath_Web_SDK** in a secure and efficient manner, covering everything from installation to API usage and error handling.
