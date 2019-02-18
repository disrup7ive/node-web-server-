# node-Web-Server & Application Deployment
•	Hello Express
•	Creating a Web Server
•	Rendering Templates with Data
•	Advanced Templating
•	Express Middleware
•	Adding Version Control (Git)
•	Setting Up GitHub & SSH Keys
•	Deploying Your Apps
•	Adding a New Feature and Deploying

    - Create Node web servers
    - Employ version control in your app
    - Deploy apps
    - Will be using Express

    Hello Express
        - Can go to URL rather than running in a terminal
        - Express is most popular npm library
        - Express
            - Installed with 'npm install express --save'
            - imported with require('express');
            - Create an app with 'var app = express();'
            - Get requests
                - handle get requests with app.get(url, function(req, res));
                    - req == request -- A ton of info about request coming in, including headers used, body info, header remaining
                    - res == response -- Includes a bunch of methods for us, can respond to http request any way we like. Customize data we send back, set HTTP status codes, etc.
                - We'll start out with response.send, sends some data back.
                - Set routes using app.get('route string', (req, res) => { handler function });
                    - handler function can be arrow function
                    - Can set as many routes as we like before app.listen();
            - Set app to listen
                -   app.listen(port)
                    - port 3000 is common dev port
                    - Will want to customize to server for prod
            - res.send('string');
                - sends info back
                - Can include HTML!
                - Can be JSON object!

    Creating a Web Server
        - Set up a static directory so custom routes don't need to be set up for every additional file
        - Create static assets we can serve up!
            - Created public folder and help.html page to be served
            - Going to use express middleware using app.use(middleware);
            - We use app.use(express.static(__dirname + '/public'));
                - requires absolute path, so we use __dirname arg to get path up to current dir and concatenate the file we want to use
            - Can add second arg function to app.listen() to do something like say server is up in console.

    Rendering Templates with Data
        - Use a templating engine, Handlebars
        - install with 'npm install hbs --save'
        - use with 'require('hbs');'
        - Integrate with Express with app.set(key, value);
            - app.set('view engine', 'hbs');
        - Create views in views file with .hbs extension
        - Render using 'res.render()' instead of 'res.send()'
            - res.render('page.hbs', {
                key: 'value'
            });
                - page.hbs can be replaced with whatever page goes in there
                - second arg is an object filled with data to reference inside the template
                - Reference in template with '{{key}}' and it will render with the value.

    Advanced Templating
        - Both pages currently have same footer and header structure.
        - We could use a partial!
        - hbs.registerPartials('absoluteDirectory');
            - Registers the directory where your partials will be stored
            - Must be absolute path, so '__dirname + directory' may be helpful
        - Nodemon won't monitor .hbs files by default
            - can use -e flag to specify extensions like '-e js,hbs'
        - Can use template vars in partials, they pass through
        - Handlebars Helper
            - Some functions can be registered to generate output.
            - Can move currentYear to a helper
            -   hbs.registerHelper('getCurrentYear', () => {
                    return new Date().getFullYear()
                })
                - first arg is name of helper
                - second arg is function for helper
            - can then use helper like template var in '{{getCurrentYear}}'
            - Can pass args to helpers by adding them with a space
                -   hbs.registerHelper('screamIt', (text) => {
                        return text.toUpperCase();
                    });
                -   {{screamIt welcomeMessage}}

    Express Middleware
        - Register middleware with app.use function as follows:
            -app.use((req,res,next) => {
                req is still request
                res is still response
                next tells express when the middleware function is done.
            });
        - We can do whatever we like, and call next(); to tell the server to continue
            - Log request data
            - Log response data
            - Check if a user is registered
            - etc
        - We can also not call next() and create something like a maintenace page, which will never allow a user to get to main pages
            - These are used in order, so use before static items.

    Adding Version Control
        - Using Git
            - Save project
            - Back up to GitHub
            - Deploy project to web
        - Initialize git in project folder
        - Create .gitignore
        - Make initial commit

    Setting up GitHub & SSH Keys
        - Guide on GitHub available
        - Generate a key, or use existing key
            - no ext is private key
                - never give to ANYONE
            - pub key is for services and other users
        - Create new repo
        - Add SSH instructions

    Deploying App on Heroku
        - Get Heroku account
        - Install Heroku toolbelt
        - Login using 'heroku login'
        - Add SSH key using 'heroku keys:add'
        - Test SSH connection using 'ssh -v git@heroku.com'
            - look for 'Authentication succeeded (publickey)'
        - Changes to make to code
            - port to listen == heroku env var
                -   const port == process.env.PORT || 3000;
                -   app.listen(port, () => {
                        console.log('Server is up on port 3000');
                    });
            - Script to be run from the terminal in package.json
                -   "scripts": {
                    ...
                    "start": "node server.js"
                }
                - Can now start from terminal using 'npm start'
        - Commit
        - Push
        - Deploy on Heroku
            - 'heroku create' from inside project
                - Creates new heroku app
                - Creates new remote for git repository
                    - origin remote goes to github
                    - heroku remote goes to heroku git repo
            - 'git push heroku'
                - Pushes to heroku git
            - 'heroku open'
                - Opens in default browser

    Adding a new feature and deploying
        - Add a new projects page
            - New route
            - New view
                - Title
                - Projects Page Here
            - New link in header
        - Commit
        - Push
        - Heroku push
