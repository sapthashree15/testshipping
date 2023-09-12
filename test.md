<img src="images/IDSNlogo.png" width="300">

::page{title="Final Hands-on lab: Shipping Calculator Application"}

## Estimated Time: 60 minutes

## Objectives:

In this lab, you will build a Simple Shipping Calculator Application that consists of an Admin Login page for authentication. Once authenticated, admin can access the Shipping Calculator dashboard to manage freight rates. The application will allow admin to add new freight rates by inputting package sizes and corresponding shipping rates. The entered freight rates will be displayed in a table format for easy reference.

## Exercise 1: Setup a React Project

**Fork the Git repository to have the react code you need to start**

**1.** Go to the project repository on this [link](https://github.com/ibm-developer-skills-network/utckx-shipping-calculator.git) which has the partially developed code for react code.

**2.** Create a fork of the repository into your own. *You will need to have a github account of your own to do so.*

<img src="images/Fork-repo.png" width="75%"/> 

**3.** Go to your repository and copy the clone url.

<img src="images/repo-url.png" width="75%"/> 

**4.** In the Visual studio code, Open a terminal window by using the menu in the editor: Terminal > New Terminal.

<img src="images/vs-code-starting-img.png" width="75%">

<img src="images/new-terminal.png" width="75%"/> 

**5.** Modify the directory location where you intend to store your project on your local machine, as demonstrated below.

<img src="images/cd.png" width="75%"/> 

**6.** Clone the forked repository by running the command given below:

```
git clone <your_repo_name>
```

<img src="images/git-clone.PNG" width="75%"/> 

**7.** This will clone the repository with Shipping Calculator app files in your home directory in the Visual Studio Code. Check the same with the following command.

```bash
ls
```

**8.** Change to the project directory and check its contents.

```bash
cd utckx-shipping-calculator && ls
```

**9.** All packages required to be installed are listed in package.json. Run npm install -s, to install and save those packages.

```bash
npm install -s
```

**10.** Here is the file organization or structure of the Simple Shipping Calculator Application.

<img src="images/file-explorer.png" width="75%"/>


## Exercise 2: Put the UI Components in Place.

<img src="images/shipping-app-1.png" width="75%"/>

<img src="images/shipping-2.png" width="75%"/>


The image above displays the Simple Shipping Calculator Application page you will be developing in this final project.

It has the following four components:
- Chart.js
- LoginForm.js
- ShippingCalculator.js
- DestinationList.js

This folder contains data files which will be used in this application.
- India-city-names.json - A JSON file containing a list of India cities.
- US_States_and_Cities.json - A JSON file containing a list of US states and cities.


In this final project, you will leverage the power of React.js to create the Shipping Calculator Application with Admin Login. React.js facilitates the management of state, UI rendering, and event handling, making it an ideal choice for building interactive and dynamic web applications.

#### Details of the three components:

**ShippingCalculator Component:** The main component that manages admin authentication, adding freight rates, and displaying the rates in a chart. It tracks the login status, freight rates, package size, destination, rate, and selected currency.

**LoginForm Component:** Responsible for displaying a login form, allowing admin to input their credentials. It triggers the login process when the login button is clicked and communicates the login status to the ShippingCalculator component.

**Chart Component:** Receives freight rates and currency as props and displays them in a tabular format. It formats the shipping rates with the appropriate currency symbol (USD or INR) and shows the package weight, destination, and shipping rate in the chart.

**DestinationList Components:** This file contains data that provides options for city and state selections in the ShippingCalculator component. It is used to dynamically generate the dropdown options based on the selected package weight unit (KG or LB). The file is typically located in the src directory, within the data subdirectory.


**Task 1**: Implement the LoginForm Component

1. Within the `LoginForm` component, the essential dependencies and the React hook `useState` have been imported from the React library.

2. We have already declared a state variable for the username now, utilize the useState hook to declare a state variable for the password.

```
const [password, setPassword] = useState('');
```

3. The handleLogin function is responsible for handling the login process. In a real-world application, this is where authentication would typically occur. However, for simplicity, we are using hardcoded credentials, where the username is "admin" and the password is "admin." If the provided username and password match these values, the onLogin function is called. Otherwise, an alert is triggered indicating that the username or password is invalid.

```
const handleLogin = () => {
    // In a real application, you would perform authentication here
    // For simplicity, we're using hardcoded username "admin" and password "admin"
    if (username === 'admin' && password === 'admin') {
      onLogin();
    } else {
      alert('Invalid username or password');
    }
  };

```
4. This code snippet comprises an Admin Login form, which features a prominent "Admin Login" heading. The form includes input fields for entering both a username and a password. These input fields are closely connected to the `username` and `password` state variables, ensuring that any user input directly updates these variables. 

- To further facilitate interaction, `onChange` event handlers are in place for both input fields, monitoring user input and dynamically modifying the associated state variables. Finally, the form is equipped with a "Login" button, and when clicked, it activates the `handleLogin` function to initiate the login process.

- The code for the username input field is already provided. Now, you should complete the password input field and add a login button to initiate the login process.

```
<input
type="password"
placeholder="Enter password"
value={password}
onChange={(e) => setPassword(e.target.value)}
/>
<button onClick={handleLogin}>Login</button>

```

5. The finalized LoginForm.js should resemble the following:

<details>
<Summary>Click to view the code</Summary>
```
import React, { useState } from 'react';

const LoginForm = ({ onLogin }) => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = () => {
    // In a real application, you would perform authentication here
    // For simplicity, we're using hardcoded username "admin" and password "admin"
    if (username === 'admin' && password === 'admin') {
      onLogin();
    } else {
      alert('Invalid username or password');
    }
  };

  return (
    <div>
      <h2>Admin Login</h2>
      <input
        type="text"
        placeholder="Enter username"
        value={username}
        onChange={(e) => setUsername(e.target.value)}
      />
      <input
        type="password"
        placeholder="Enter password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={handleLogin}>Login</button>
    </div>
  );
};

export default LoginForm;
```
</details>

<img src="images/shipping-app-1.png" width="75%"/>

**Task 2**: Create the ShippingCalculator Component

1. In the ShippingCalculator component, we've imported various dependencies and data sources to support its functionality. These include React, the Chart component, LoginForm, a custom styles file, the react-select component, and data for cities in India and the United States.

- The component manages several state variables, including isLoggedIn to track the user's login status, freightRates to store shipping rates, packageSize to capture package size information, destination to select the delivery destination, rate for calculating shipping rates, and packageWeightUnit to specify the unit of package weight as kilograms (kg) by default.

2. Within the code, the `handleLogin` function is designed to set the `isLoggedIn` state to `true`, indicating a successful login.

Additionally, there's an `addFreightRate` function which serves to add freight rate data. It performs input validation, checking if the `packageSize`, `destination`, and `rate` fields are all filled in. If any of these fields are empty, it triggers an alert requesting the user to fill in all the fields before proceeding.

```
const handleLogin = () => {
    setIsLoggedIn(true);
  };

  const addFreightRate = () => {
    if (!packageSize || !destination || !rate) {
      alert('Please fill in all the fields.');
      return;
    }
```

3. In this code snippet, a `currency` variable is determined based on the `packageWeightUnit`. If the `packageWeightUnit` is 'kg', the currency is set to 'inr' (Indian Rupees); otherwise, it's set to 'usd' (US Dollars).

- Next, a `newRate` object is created, encapsulating information such as `packageSize`, `destination`, `rate`, `currency`, and `packageWeightUnit`. This object represents a new freight rate entry.

- To update the `freightRates` state, the `newRate` is added to the existing array of freight rates using the spread operator (`...`) to maintain the previous rates.

- After adding the rate, the `packageSize`, `destination` (reset to `null`), and `rate` fields are cleared, ensuring a clean slate for the next rate entry.

4. The getCityOptions function generates city options based on the selected packageWeightUnit. If the unit is 'kg', it retrieves city data from the IndiaCities object, and if it's 'lb' (pounds), it fetches city data from the USCities object. The function then transforms the data into an array of objects with value and label properties for use in the react-select component.

For example, if the unit is 'kg', it maps the cities in India to an array of objects where each object has a value and label property representing the city and country. The resulting array contains city options suitable for populating a dropdown or selection list in the user interface.

<details>
<Summary>Click to view the code</Summary>
```
const getCityOptions = () => {
    if (packageWeightUnit === 'kg') {
      return Object.keys(IndiaCities).flatMap((country) =>
        IndiaCities[country].map((city) => ({ value: `${city}, ${country}`, label: `${city}, ${country}` }))
      );
    } else {
      return Object.keys(USCities).flatMap((state) =>
        USCities[state].map((city) => ({ value: `${city}, ${state}`, label: `${city}, ${state}` }))
      );
    }
  };

```
</details>


5. Use conditional rendering to display different components based on the isLoggedIn state:

- If the admin is not logged in, render the LoginForm component with the handleLogin function as a prop.
- If the admin is logged in, render the main shipping calculator interface.
- Display the "Admin Dashboard" heading, the form to add freight rates, and the Chart component.

6. Implement the JSX for the form to add freight rates:

- Create a form element.
- Inside the form, render input elements for packageSize and rate.
- Use the className attribute to apply CSS classes for styling the input elements.
- Implement the dropdown for package weight unit ('kg' or 'lb') using the select element and options.
- Use the Select component for the city/state dropdown, passing the city options from the getCityOptions function.
- Bind the value and onChange attributes of the input elements and the Select component to the respective state variables.

7. Attach event handlers to the input elements and the "Add Rate" button:

- Use the onChange event to update the packageSize, rate, and packageWeightUnit state variables as the admin enters data.
- Implement the addFreightRate function as the event handler for the "Add Rate" button click.

8. Display the Chart component to visualize the added freight rates:

- Pass the freightRates array as a prop to the Chart component.


8. Below is the finalized code for the ShippingCalculator.js file.

<details>
<Summary>Click to view the code</Summary>
```
import React, { useState } from 'react';
import Chart from './Chart';
import LoginForm from './LoginForm';
import '../styles.css'; // Import the styles.css file
import Select from 'react-select'; // Import the react-select component
import IndiaCities from '../data/India-city-names.json'; // Import the India city data
import USCities from '../data/US_States_and_Cities.json'; // Import the US city data

const ShippingCalculator = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [freightRates, setFreightRates] = useState([]);
  const [packageSize, setPackageSize] = useState('');
  const [destination, setDestination] = useState(null);
  const [rate, setRate] = useState('');
  const [packageWeightUnit, setPackageWeightUnit] = useState('kg');

  const handleLogin = () => {
    setIsLoggedIn(true);
  };

  const addFreightRate = () => {
    if (!packageSize || !destination || !rate) {
      alert('Please fill in all the fields.');
      return;
    }

    const currency = packageWeightUnit === 'kg' ? 'inr' : 'usd';

    const newRate = { packageSize, destination: destination.label, rate: parseFloat(rate), currency, packageWeightUnit};
    setFreightRates([...freightRates, newRate]);

    setPackageSize('');
    setDestination(null); // Reset the destination to null after adding the rate
    setRate('');
  };

  // Get the appropriate cityOptions based on the selected package weight unit
  const getCityOptions = () => {
    if (packageWeightUnit === 'kg') {
      return Object.keys(IndiaCities).flatMap((country) =>
        IndiaCities[country].map((city) => ({ value: `${city}, ${country}`, label: `${city}, ${country}` }))
      );
    } else {
      return Object.keys(USCities).flatMap((state) =>
        USCities[state].map((city) => ({ value: `${city}, ${state}`, label: `${city}, ${state}` }))
      );
    }
  };

  return (
    <div>
      {!isLoggedIn ? (
        <LoginForm onLogin={handleLogin} />
      ) : (
        <div className="shipping-calculator-container">
          <h1>Admin Dashboard</h1>
          <div>
            <h2>Add Freight Rate</h2>
            <form>
              <div className="package-weight-container">
                <input
                  type="text"
                  placeholder="Package Weight"
                  value={packageSize}
                  onChange={(e) => setPackageSize(e.target.value)}
                  className="package-size-input"
                />
                <select
                  className="select"
                  value={packageWeightUnit}
                  onChange={(e) => setPackageWeightUnit(e.target.value)}
                >
                  <option value="kg">KG</option>
                  <option value="lb">LB</option>
                </select>
              </div>
              <div className="city-state-dropdown">
                <Select
                  options={getCityOptions()} // Use the filtered cityOptions based on package weight unit
                  value={destination}
                  onChange={(selectedOption) => setDestination(selectedOption)}
                  placeholder="Select City and State"
                />
              </div>
              <br />
              <div className="shipping-rate-input">
                <input
                  type="text"
                  placeholder="Shipping Rate"
                  value={rate}
                  onChange={(e) => setRate(e.target.value)}
                />
                <select
                  className="select2"
                  value={packageWeightUnit === 'kg' ? 'inr' : 'usd'}
                  onChange={(e) => setPackageWeightUnit(e.target.value === 'inr' ? 'kg' : 'lb')}
                >
                  <option value="inr" className="inr-option">
                    INR
                  </option>
                  <option value="usd" className="usd-option">
                    USD
                  </option>
                </select>
              </div>
              <button type="button" onClick={addFreightRate}>
                Add Rate
              </button>
            </form>
          </div>
          <Chart freightRates={freightRates} packageWeightUnit={packageWeightUnit} />
        </div>
      )}
    </div>
  );
};

export default ShippingCalculator;

```
</details>

<img src="images/Task-2.png" width="75%"/>


**Task 3**: Display Destination Options

1. DestinationList file contains city options for use in a shipping calculator from JSON files containing data for India and the United States. It iterates through the data, creating an array of city objects with corresponding values and labels. These objects are structured with the city name and its associated country, making them suitable for use in select dropdowns or similar UI components. The resulting cityOptions array serves as a valuable resource for populating city selection elements within the shipping calculator component.

```
Object.keys(USCities).forEach((country) => {
  USCities[country].forEach((city) => {
    cityOptions.push({ value: `${city}, ${country}`, label: `${city}, ${country}` });
  });
});

```
<img src="images/destinationlist.png" width="75%"/>

**Task 4**: Implement the Chart Component

1. This React component, known as Chart, is designed to display a shipping rate chart based on the provided freightRates and packageWeightUnit props. The chart is encapsulated within a div element with the class name "chart-container" for styling purposes.

2. The table consists of a header row with three columns: "Package Weight," "Destination," and "Shipping Rate." The body of the table is dynamically generated based on the freightRates prop. For each freight rate in the array, a row is created with three cells:

- The first cell displays the package size and weight unit (e.g., "10 kg") using the packageSize and packageWeightUnit properties from the rate object.

- The second cell shows the destination.

- The third cell displays the shipping rate, which is formatted as either a USD or INR amount based on the currency property of the rate object. The rate is presented with two decimal places.

The freightRates prop should be an array of objects, each containing information about a specific shipping rate. The packageWeightUnit prop determines the unit used for displaying package weight in the chart.


3. In the first `<td>`, utilize the `${rate.packageSize} ${packageWeightUnit}` expression to present both the package size and its corresponding weight unit. This formatting will result in a clear representation, such as "10 kg" or "20 lb," effectively conveying the package's weight.

4. In the second `<td>`, there's a straightforward directive to display the `rate.destination` value. This column straightforwardly reveals the destination associated with each individual shipping rate, allowing users to easily identify the intended delivery location.

5. In the third `<td>`, employ the `${rate.currency === 'usd' ? '$' : '₹'}${rate.rate.toFixed(2)}` expression to showcase the shipping rate. This expression guarantees that the rate is correctly presented, featuring the appropriate currency symbol ('$' for USD or '₹' for INR) and ensuring that it is rounded to two decimal places. This precise formatting is crucial for transparency when presenting shipping costs to users.

```
<td>{rate.packageSize} ({rate.packageWeightUnit})</td>
<td>{rate.destination}</td>
<td>{`${rate.currency === 'usd' ? '$' : '₹'}${rate.rate.toFixed(2)}`}</td>

```

6. Click here to view the complete code.

<details>
<summary>Solution</summary>
```
import React from 'react';

const Chart = ({ freightRates, packageWeightUnit }) => {
  return (
    <div className="chart-container">
      <h2>Shipping Rate Chart</h2>
      <table>
        <thead>
          <tr>
            <th>Package Weight</th>
            <th>Destination</th>
            <th>Shipping Rate</th>
          </tr>
        </thead>
        <tbody>
          {freightRates.map((rate, index) => (
            <tr key={index}>
              <td>{rate.packageSize} ({rate.packageWeightUnit})</td>
              <td>{rate.destination}</td>
              <td>{`${rate.currency === 'usd' ? '$' : '₹'}${rate.rate.toFixed(2)}`}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default Chart;

```
</details>

Once the Chart component is defined, it can be integrated into the shipping calculator interface to provide users with a clear and organized view of shipping rates.

<img src="images/chart.png" width="75%"/>


**Task 4**: Styling and Additional Features

1. Style the application to make it visually appealing. You can use CSS classes from the provided **styles.css** file or add your own styles.

2. Add error handling to the login form and freight rate inputs, so users are prompted if any of the fields are empty or invalid.

3. Include additional features such as sorting the freight rates table based on different columns, filtering rates, or displaying a chart visualization of the rates.

## Launch and view your react app on the browser

1. Make sure you are in the utckx-shipping-calculator directory and run the server using the following command.

    npm start

2. Once you run the above command the server will start in the default browser in port 3000 as shown below.

<img src="images/finaloutput1.png" width="75%"/>

<img src="images/finaloutput2.png" width="75%"/>



## Commit and push your local code to your remote git repository


1. **Check Git Status:**
   Before committing any changes, check the status of your local repository to see what files have been modified and need to be committed. Open your terminal or command prompt and navigate to your project directory.

   ```bash
   git status
   ```

2. **Add Changes to the Staging Area:**
   Use the `git add` command to add the files you want to commit to the staging area. The `.` adds all changes.

   ```bash
   git add .
   ```

3. **Set Your Git User Email and Name:**
   If you haven't already configured your email and name globally, use the `git config` command to set them.

   ```bash
   git config --global user.email "your_email@example.com"
   git config --global user.name "Your Name"
   ```

   Replace `"your_email@example.com"` with your email and `"Your Name"` with your name.

4. **Commit Changes:**
   Commit the changes in the staging area with a meaningful commit message using the `git commit` command. Replace `'Completed the code'` with a concise description of your changes.

   ```bash
   git commit -m 'Completed the code'
   ```

5. **Push to Remote Repository:**
   Push your committed changes to the remote Git repository using the `git push` command. Replace `origin` with the name of your remote repository (often "origin") and `branch_name` with the name of the branch you want to push (e.g., "main" or "master").

   ```bash
   git push origin branch_name
   ```

   If you are pushing to the default branch (e.g., "main"), you can simply use:

   ```bash
   git push origin
   ```

Congratulations! You have successfully completed the Shipping Calculator application lab. With this application, users can log in, add freight rates, and view the rates in a chart.

## Author(s)

Sapthashree K S

**Change Log**

| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2023-07-31 | 1.0 | Sapthashree K S | Initial version created |

## <h3 align="center"> © Skills Network 2023. All rights reserved. <h3/>

