# Implementation of security headers in JavaScript

```
import helmet from 'helmet'
import csp from 'helmet-csp'

// security headers
app.use(helmet.frameguard()) // sameorigin
app.use(helmet.noSniff())
app.use(helmet.xssFilter())  // 1; mode=block 
app.use(helmet.referrerPolicy({ policy: 'no-referrer-when-downgrade' }))
app.use(csp({
  // Specify directives as normal.
  directives: {
    // defaultSrc is used in any directive that is not explicitly listed. Keep it strong.
    defaultSrc: ["'self'"],
    // sandbox applies restrictions to the actions on a page.
    sandbox: ['allow-scripts'], // 'allow-forms',
    objectSrc: ["'none'"],
    upgradeInsecureRequests: true,
    workerSrc: false  // This is not set.
  },
 
  // This module will detect common mistakes in your directives and throw errors
  // if it finds any. To disable this, enable "loose mode".
  loose: false,
 
  // Set to true if you only want browsers to report errors, not block them.
  // You may also set this to a function(req, res) in order to decide dynamically
  // whether to use reportOnly mode, e.g., to allow for a dynamic kill switch.
  reportOnly: false,
 
  // Set to true if you want to blindly set all headers: Content-Security-Policy,
  // X-WebKit-CSP, and X-Content-Security-Policy.
  setAllHeaders: true,
 
  // Set to true if you want to disable CSP on Android where it can be buggy.
  disableAndroid: false,
 
  // Set to false if you want to completely disable any user-agent sniffing.
  // This may make the headers less compatible but it will be much faster.
  // This defaults to `true`.
  browserSniff: true
}))

app.set('port', process.env.PORT || 8000)
app.disable('x-powered-by')
app.disable('server')
```
