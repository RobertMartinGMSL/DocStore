# Run Website locally against WIP environment

1. Add to `C:\Windows\System32\drivers\etc\hosts`
   - `127.0.0.1 esp.awstest.gasops.co.uk`
   - Remember to remove when done, otherwise localhost will be pointing at WIP permanently
1. Create dev environment file
   - Create `...\Gmsl.Esp.Website\.env.development`
   - Add 
     - `HTTPS=true`
     - `PORT=443`
1. Edit debug config file
   - Copy content from `...\Gmsl.Esp.Website\.config.wip`
   - Over existing content of `...\Gmsl.Esp.Website\.config.debug`
1. Edit api/authenticationApi.js
   - Edit `...\Gmsl.Esp.Website\src\api\authenticationApi.js`
   - Ensure `./authenticationApi` ends with `.prod` for all ocurrences
1. Open in terminal and run website only
   - Open Terminal at `...\Gmsl.Esp.Website`
   - run `npm run start:dev`
