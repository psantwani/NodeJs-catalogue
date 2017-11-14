1. Use [nconf](https://github.com/indexzero/nconf) to load configurations on various environments
2. Use try-catch in sync code only
3. A process control system should restart the application when it crashes, like: supervisord or monit
4. Using [JSCS](https://github.com/jscs-dev/node-jscs/tree/master/presets) to check your code style against that of companies like airbnb, google, etc. (industry standards)
5. Use JS over JSON for configurations (more flexibility)
6. ```var myModule = require('../../../../lib/myModule');``` can be written as ```require('myModule')``` using NODE_PATH like ```"start": "NODE_PATH=lib node index.js"``` script
7. Dependency injection is really helpful when it comes to testing
8. You should not try to listen with Node on port 80 - to do so you would need superuser rights, but thats not a good idea. Still if you want to run it, run your app on any port above 1024, then put a reverse proxy like nginx in front of it.
9. Stubs to test - Stubs are functions/programs that simulate the behaviours of components/modules