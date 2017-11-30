## Formik and Yup 

**React Forms Library and Object Validator**

Formik is a great library that abstracts much of the work of creating Forms and Inputs in React.

Yup is a JavaScript object schema validator and object parser, but in formik we can use it to verify input fields!

[Formik Docs](https://github.com/jaredpalmer/formik) - Scroll down to find the Table of Contents

[Link To Formik API Docs](https://github.com/jaredpalmer/formik#api)

Here is a full sample app to get started:

```javascript
import React from 'react';
import { render } from 'react-dom';
import { withFormik, Form, Field } from 'formik';
import Yup from 'yup';

const App = (props) => (
  /* Pulling out from props
    values,
    errors,
    touched,
    isSubmitting
  */

  <Form>
    <div>
      { props.touched.email && props.errors.email && <p>{props.errors.email}</p> }
      <Field type="text" 
        name="email" 
        placeholder="Email" 
      />
    </div>
    <div>
      {props.touched.password && props.errors.password && <p>{props.errors.password}</p>}
      <Field type="password"
        name="password"
        placeholder="Password"
      />
    </div>
    <label>
      <Field type="checkbox" 
        name="newsletter"
        checked={props.values.newsletter}
      />
      Join Newsletter
    </label>
    <Field component="select" name="plan">
      <option value="free">Free</option>
      <option value="premium">Premium</option>
    </Field>
    <button disabled={props.isSubmitting}>Submit</button>
  </Form>
);

const FormikApp = withFormik({
  mapPropsToValues({ email, password, newsletter, plan }) {
    return {
      email: email || '',
      password: password || '',
      newsletter: newsletter || '',
      plan: plan || 'premium'
    }
  },
  validationSchema: Yup.object({
    email:Yup.string().email('Not a valid email').required('email is required'),
    password: Yup.string().min(6, 'Password gotta be 6 long').required('password required')
  }),
  handleSubmit(values, formikBag) {
    const { setErrors, resetForm, setSubmitting } = formikBag;

    setTimeout(() => {
      if(values.email === 'm@m.com') {
        setErrors({email: 'Email already Taken'})
      } else {
        resetForm();
      }
      setSubmitting(false);
    }, 2000)
    console.log(values);
    console.log(formikBag);

  }
})(App);

render(<FormikApp initObj="Aw Hell"/>, document.getElementById('root'));

```

## Overview

In the above example, I have used the Formik supplied components of **Form** and **Field**, but you do not need to use these, you can you the standard HTML form and field types like input, select, etc.  However, by using **Field** you get to abstract away the need for things like **value** and **onChange** as these are automatically supplied by matching on the **name**.

Formik does all it's magic via an HOC and by passing a configuration object to the HOC.

```javascript
const FormikApp = withFormik({
  mapPropsToValues() {},
  validationSchema: {}, //usually Yup.object({})
  handleSubmit() {},
  ...
})(MyComponent);

export default FormikApp;
```





