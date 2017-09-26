
$ mkdir package-to-import
$ cd package-to-import/
$ yarn init -y

$ less package.json
{
  "name": "package-to-import",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}

$ cat > index.js
module.exports = function() {
  console.log('print from package-to-import');
}
^D


----------------------------------

$ mkdir ../main-module
$ cd ../main-module
$ yarn init -y
$ less package.json
{
  "name": "main-module",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
}

$ yarn add file:../package-to-import
...
...
success Saved 1 new dependency.
└─ package-to-import@1.0.0


$ less package.json
{
  "name": "main-module",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "package-to-import": "file:../package-to-import"    <------[!]
  }
}

$ tree ../
../
├── main-module
│   ├── index.js
│   ├── node_modules
│   │   └── package-to-import   <--------[!]
│   │       ├── index.js
│   │       └── package.json
│   ├── package.json
│   └── yarn.lock
└── package-to-import
    ├── index.js
    └── package.json

-------------

$ cat > index.js
var importedPackage = require('package-to-import');
importedPackage();
^D

$ node index.js
print from package-to-import


-------------

$ vim ../package-to-import/index.js
$ less ../package-to-import/index.js
module.exports = function() {
  console.log('print from package-to-import ***EDIT***');
}                                           ^^^^^^^^^^

$ node index.js
print from package-to-import
                             ^^^^^^^---[!]


$ yarn upgrade package-to-import
       ^^^^^^^
...
...
success Saved 1 new dependency.
└─ package-to-import@1.0.0

$ node index.js
print from package-to-import ***EDIT***
                             ^^^^^^^^^^


----------


https://stackoverflow.com/questions/40102686/how-to-install-package-with-local-path-by-yarn-it-couldnt-find-package/40116358#40116358
https://stackoverflow.com/questions/8088795/installing-a-local-module-using-npm/8089029#8089029


