![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Sign up Confirmation Email

## Introduction

![image](https://user-images.githubusercontent.com/23629340/37091320-032a2cb0-2208-11e8-8b73-27060f1960c3.png)

Every time you create an account on a new service, you're asked to confirm your email address by clicking on a link that is sent to the you. This is a great way stop people trying to create disposable or spammy accounts.

In this lab, we will do the same exact thing - create an app that allows users to sign up, but their status will be by default set to `pending_confirmation`.

After creating an account, the user should receive a confirmation email with a clickable link that includes a confirmation "token" in the query. After navigating to this link, their status should be changed to `active`. We will use **nodemailer** for this!

## Requirements

- Fork this repo
- Then clone this repo

## Submission

- Upon completion, run the following commands:

```
$ git add .
$ git commit -m "done"
$ git push origin master
```

- Create Pull Request so your TAs can check up your work.

## Instructions

### Iteration 1 - User Model

First, we need to modify the `User` model. Inside of the `models` folder, you will find a `user.js` file. We already have the `email` and `password` fields on the user schema, so we just need to add the following properties:

- **`status`** - Should be a string, and you should only accept the values _"pending_confirmation"_ or _"active"_ (you should use the `enum` validator). By default, when a new user is created, their status should be set to _"pending_confirmation"_.
- **`confirmationToken`** - here we will store a confirmation token. Every users should have an unique token.

### Iteration 2 - Sign Up Process

#### Adding the new fields

When the user submits the sign up form, you should create a new user document with and store the following values:

- **email** - From the `req.body`;
- **passwordHash** - After hashing the value of the `password` field from the `req.body`;
- **confirmationToken** - A confirmation token generated right before creating the document. You can either use `Math.random().toString()` to achieve this, an npm package that generates random ids,  or use the following function:

```js
const generateRandomToken = length => {
  const characters = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
  let token = '';
  for (let i = 0; i < length; i++) {
    token += characters[Math.floor(Math.random() * characters.length)];
  }
  return token;
};
```

Now, you have to store the token in the `confirmationToken` field.

#### Sending the email

After creating the user, you should send an email to the address the user inserted on the `email` field. Remember to use **nodemailer** for this. You should include the following URL in the email:

`http://localhost:3000/authentication/confirm-email?token=THE-CONFIRMATION-CODE-OF-THE-USER`

### Iteration 3 - Confirmation Route

When a user follows the confirmation link sent by email, we should look up the user document that contains a `confirmationToken` with the value of `req.query.token`, and update the corresponding user document to have the value `active` in status property.

To achieve this, you should create a route handler for requests to the `/authentication/confirm-email` endpoint. After a successfull confirmation, render a `confirmation.hbs` view, letting the user know that everything went perfect, or displaying an error.

### Iteration 4 - Profile View

Finally, you should to create a `profile.hbs` template, where you should to render the `name` and the `status` of the user. The should be able to see this view by visitin `/profile`.

### Bonus! Styling the Email

Sending the email that contains only the URL is super boring! Feel free to style it better.

![image](https://user-images.githubusercontent.com/23629340/37099024-ab0d7c9a-221f-11e8-9458-49f813437e2c.png)

Happy Coding! 💙
