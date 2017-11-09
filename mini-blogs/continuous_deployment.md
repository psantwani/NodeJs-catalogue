# Continuous integration
Merging development work with master constantly. This helps catching issues early and prevent integration hell. Automated tests run here.

# Continuous delivery
Delivery of code to an environment (QA or customers). After review and approval, the code lands into production.

# Continuous deployment
Automated testing of changes in continuous delivery followed by auto-depolyment to production. 

Steps
- Developer commits code to source control 
- Triggers build in continuous integration servers (Jenkins, Travis, Codeship, Strider)
- Deploy to prod if all tests pass

Tools
- Use [features-toggles](https://www.npmjs.com/package/feature-toggles) to control release of unfinished features to prod
- Set commit hooks on CI tools to avoid regular polling to git/svn

# CI Builds
A build does the following steps
- Installing npm dependencies
- run unit tests (Mocha/Jasmine with chai)
- build css/js assets
- run end-to-end tests
 - Hippie, [Supertest](https://github.com/visionmedia/supertest) for API end-to-end testing
 - BrowserStack for front-end testing
- Create artifacts (all the files need to run your app including node_modules, so you don't have to build on prod servers again)

# Deploy 
Tools like Ansible, Chef, Puppet can automate the process below:
- Download latest artifact
- Unpack to new directory
- Restart node app

Note: Keep a rollback script handy.