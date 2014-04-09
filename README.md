## Stormpath Passport Strategy

Passport authentication strategy for username & password authentication via Stormpath.

### Express.js example

Head over to: https://github.com/stormpath/passport-stormpath-example

### General example

```javascript

var passport = require('passport');
var stormpath = require('stormpath');
var StormpathStrategy = require('passport-stormpath');

var spClient;

spClient = new stormpath.Client({
    apiKey: new stormpath.ApiKey(
        process.env['STORMPATH_API_KEY_ID'],
        process.env['STORMPATH_API_KEY_SECRET']
    )
});

spClient.getApplication(
    process.env['STORMPATH_APP_HREF'],
    function(err,app){
        if(err){
            throw err;
        }
        passport.use(new StormpathStrategy({spApp:app}));
    }
);

passport.serializeUser(function(user, done) {
    done(null, user.href);
});

passport.deserializeUser(function(userHref, done) {
    spClient.getAccount(userHref,function(err,account){
        done(err,account);
    });
});
```
