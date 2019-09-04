# How to create Cypress Project
See link for more details
```
https://docs.cypress.io/guides/getting-started/installing-cypress.html
```

1. Ensure npm is installed on your system
    * The simplest way to install it is to install Node.js
      * includes npm (node package manager)
2. Create project folder
    * e.g. C:\Projects\CypressProject
3. Change working directory to project folder
```
cd C:\Projects\CypressProject
```
3. Initialise npm into project
```
npm init -y
```
4.  Install Cypress into project
```
npm install cypress
```
5. Initialise git repository
    * Add `node_modules` to .gitignore
      * contains npm packages which should be reinstalled in future using `npm i`, rather than stored in source control
```
git init
```
6. Run Cypress from project
```
npx cypress open
```
7. Run Cypress using the opened browser
    * This includes example tests