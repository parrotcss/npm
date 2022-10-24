const runScript = require('@npmcli/run-script')

runScript({
  // required, the script to run
  event: 'install',

  // extra args to pass to the command, defaults to []
  args: [],

  // required, the folder where the package lives
  path: '/path/to/package/folder',

  // optional, these paths will be put at the beginning of `$PATH`, even
  // after run-script adds the node_modules/.bin folder(s) from
  // `process.cwd()`. This is for commands like `npm init`, `npm exec`,
  // and `npx` to make sure manually installed  packages come before
  // anything that happens to be in the tree in `process.cwd()`.
  binPaths: [
    '/path/to/npx/node_modules/.bin',
    '/path/to/npm/prefix/node_modules/.bin',
  ]

  // optional, defaults to /bin/sh on unix, or cmd.exe on windows
  scriptShell: '/bin/bash',

  // optional, defaults to false
  // return stdout and stderr as strings rather than buffers
  stdioString: true,

  // optional, additional environment variables to add
  // note that process.env IS inherited by default
  // Always set:
  // - npm_package_json The package.json file in the folder
  // - npm_lifecycle_event The event that this is being run for
  // - npm_lifecycle_script The script being run
  // The fields described in https://github.com/npm/rfcs/pull/183
  env: {
    npm_package_from: 'foo@bar',
    npm_package_resolved: 'https://registry.npmjs.org/foo/-/foo-1.2.3.tgz',
    npm_package_integrity: 'sha512-foobarbaz',
  },

  // defaults to 'pipe'.  Can also pass an array like you would to node's
  // exec or spawn functions.  Note that if it's anything other than
  // 'pipe' then the stdout/stderr values on the result will be missing.
  // npm cli sets this to 'inherit' for explicit run-scripts (test, etc.)
  // but leaves it as 'pipe' for install scripts that run in parallel.
  stdio: 'inherit',

  // print the package id and script, and the command to be run, like:
  // > somepackage@1.2.3 postinstall
  // > make all-the-things
  // Defaults true when stdio:'inherit', otherwise suppressed
  banner: true,
})
  .then(({ code, signal, stdout, stderr, pkgid, path, event, script }) => {
    // do something with the results
  })
  .catch(er => {
    // command did not work.
    // er is decorated with:
    // - code
    // - signal
    // - stdout
    // - stderr
    // - path
    // - pkgid (name@version string)
    // - event
    // - script
  })
