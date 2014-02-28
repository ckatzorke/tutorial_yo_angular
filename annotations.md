@annotation:tour installation 
#Installing Yeoman
Let's start off by getting the panels set up in a nice way for the tutorial.

1. Create a new panel by selecting 'View->Panels->Split horizontal' from the menu
1. Open up a Terminal window to your Codio Box from the 'Tools->Terminal' menu
1. Note you can drag the Terminal tab into any other panel
1. Adjust the vertical splitter to suit your display

Let's get started with the real stuff by installing Yeoman. In your terminal window type

    npm install -g yo

Now site back and wait. If you were to do this on your laptop on a normal ADSL connection, it would be a LOT slower.

Once the Yeoman installation is complete, you can check the version that was installed

    yo --version && bower --version && grunt --version

Press the navigation button at the top of this panel to move to the next step.

@annotation:tour
#Installing Yeoman Generators
In a traditional web development workflow, you would need to spend a lot of time setting up boilerplate code for your webapp, downloading dependencies, and manually creating your web folder structure. Yeoman generators to the rescue! Generators do the hard work for you by scaffolding out your project. Let’s install a generator for creating AngularJS projects.

Yeoman comes with a handy interactive menu, to access this menu run:

    $ yo

![run yeoman](img/yo.png)

Use your keyboard’s up / down arrow keys to navigate the menu and hit enter when ‘Install a generator’ is highlighted.

Next we need to search for a generator to install. A search for `angular` will find many generators contributed by various members of the Yeoman open source community. In this instance we want to install the ‘generator-angular’ generator.

![angular generator](img/yo-angular.png)

With generator-angular selected, hit enter to start installing the generator. This will start to install the Node packages required for the generator.

Alternatively, if you already know the name of the generator you want to use, the generator can be installed using npm as follows:

    $ npm install -g generator-angular


@annotation:tour scaffold
#Use a Generator to scaffold your App

Once a generator has been installed, you will automatically see the scren below (or it can be accessed via the Yeoman interactive menu: `$ yo`.
    
![install angular](img/yo-install-angular.png)    

###Comments
As you become more familiar with Yo, you might want to run generators directly without the use of the interactive menu:

    $ yo angular    
    
Some generators will also provide optional settings to customize your app with common developer libraries and speed up the initial setup of your development environment.

The AngularJS generator provides options to include use Sass (with Compass) and/or Twitter Bootstrap. Enter ‘n’ and ‘y’ respectively to these options.


###Keep calm and carry on
With 'Run the Angular generator' highlighted, press Enter.

![inputs](img/angular-inputs.png)

Next you are prompted to select what Angular modules you would like to include as well. Angular modules are self-contained JavaScript files with helpful functionality. For example, the ngResource module (angular-resource.js) provides interaction support with RESTful services.

You can deselect options using the spacebar. Let’s roll with the defaults. (So if you have been playing around with the spacebar, make sure that all the modules are marked as green.)

Okay, hit enter once your inputs look like you see above. Yeoman will automatically scaffold out your app, grab your dependencies, and pull in a few useful Grunt tasks for your workflow. After a short wait it will be ready.

Don't worry if you see a log error like this

    npm ERR!                                                
    npm ERR! Additional logging details can be found in:
    npm ERR!     /home/codio/workspace/npm-debug.log 
    npm ERR! not ok code 0 

@annotation:tour files
#Explore the File Tree
In the file tree on the left, take a look at what was actually scaffolded. We have:

- **app**: a parent directory for our web application
  - **index.html**: the base html file for our Angular app
  - **404.html**, **favicon.ico**, and **robots.txt:** commonly used web files so you don’t have to create them yourself
  - **bower_components**: a home for our JavaScript/web dependencies, installed by Bower
  - **scripts**: our own JS files
  - **app.js**: our main application code
  - **controllers**: our Angular controllers
  - **styles**: our CSS files
  - **views**: a place for our Angular templates
- **Gruntfile.js**, **package.json**, and **node_modules**: configuration and dependencies required by our Grunt tasks
- **test** and **karma.conf.js/karma-e2e.conf.js**: a scaffolded out test runner and the unit tests for the project, including boilerplate tests for our controllers.

@annotation:tour mods1
#Modify some files
To run properly in Codio, you'll want to open up `Gruntfile.js` in the root of your App and search for 'localhost' (somewhere around line 64) and change this

    // Change this to '0.0.0.0' to access the server from outside.
    hostname: 'localhost',
    livereload: 35729
        
to this

    // Change this to '0.0.0.0' to access the server from outside.
    hostname: '0.0.0.0',
    livereload: 4000    

Codio leaves all Ports between 1024 and 9999 at your disposal, so we need to modify the livereload port. 


@annotation:tour mods2
#Preview your App
Codio has the ability to save you lots of time by configuring the 'Run' menu (cli commands) and the 'Preview' menu. 

Click on 'Configure...' as shown in the Run menu below.

![run menu](img/run-menu.png)

Now, copy and paste the following code into the code tab to save you the hassle ...

    {
    // Configure your Run and Preview buttons here.

    // Run button configuration
      "commands": {
        "Grunt Serve": "grunt serve",
        "Grunt Test": "grunt test",
        "Grunt Build": "grunt",
        "Grunt Serve:Dist": "grunt serve:dist"
      },

    // Preview button configuration
      "preview": {
        "Yeoman Demo": "http://{{domain}}:9000/"
      }
    }

You will now see that the Run and Preview menus have been updated and look like this ...

![run updated](img/run-updated.png)

@annotation:tour preview
#Running the App
##Start Grunt
Right, we are now ready to get Grunt to serve up our content. From the Run menu, select 'Grunt Serve' (If it appears in the upper panel covering the tutorial, then drag it down to the lower panel). 

This does nothing more than save you the hassle of typing on the command line ...

    grunt serve

You should see your terminal window running `grunt serve`. If there are any issues running Grunt, just run `npm install` from the terminal to make sure all packages got correctly installed.

##Preview
Now, from the Preview menu to the right, select 'Yeoman Demo'. Again, this is just a shortcut to ...

    http://{{domain}}:9000/

You should now see the following screen appear in a new browser tab.

![allo allo](img/allo.png)

@annotation:tour livereload
#Live Reload
With both `grunt serve` still running in the background and the Preview tab still open, open up the file `app/views/main.html` and change some text somewhere. You will notice that the browser tab auto reloads the content.

@annotation:tour step1
#Create a new Template to show a ToDo list
###app/views/main.html
First, let's modify our view (views/main.html) to output our todos items as text input fields. Copy and paste the following code into the `app/views/main.html` file (replace everything).

    <div class="container">
      <h2>My todos</h2>
      <p class="form-group” ng-repeat="todo in todos">
        <input type="text" ng-model="todo" class="form-control">
      </p>
    </div>

###app/scripts/controllers/main.js
Now, let's modify the controller script by repolacing everything with ...

    'use strict';

    angular.module('workspaceApp')
      .controller('MainCtrl', function ($scope) {
        $scope.todos = ['Item 1', 'Item 2', 'Item 3'];
      });

The [ng-repeat](http://docs.angularjs.org/api/ng.directive:ngRepeat) attribute on the paragraph tag is an [Angular directive](http://docs.angularjs.org/guide/directive) that instantiates a template once per item from a collection. In our case, imagine that the paragraph element and its content is turned into a virtual rubber stamp by adding the ng-repeat attribute. For each item in the todos array, Angular will stamp out a new instance of the `<p><input></p>` HTML.

The [ng-model](http://docs.angularjs.org/api/ng.directive:ngModel) attribute is another Angular directive that works with input, select, textarea and custom controls to create a two-way data binding. In our example, it populates a text input field with the value from the current todo item in the ng-repeat loop.

Your browser should now show something like this

![preview](img/preview-1.png)

@annotation:tour step2
#Adding a ToDo
Let’s implement a way to add new todo items to the list of existing todos within the application.

Modify `app/views/main.html` by adding a form element in between the `<h2>` and `<p>` elements from the previous section. Your views/main.html should now look like this:

    <div class="container">
      <h2>My todos</h2>

      <!-- Todos input -->
      <form role="form" ng-submit="addTodo()">
        <div class="row">
          <div class="input-group">
            <input type="text" ng-model="todo" placeholder="What needs to be done?" class="form-control">
            <span class="input-group-btn">
              <input type="submit" class="btn btn-primary" value="Add">
            </span>
          </div>
        </div>
      </form>
      <p></p>

      <!-- Todos list -->
      <p class="form-group" ng-repeat="todo in todos">
        <input type="text" ng-model="todo" class="form-control">
      </p>
    </div>

This adds a form with a submit button to the top of the page. It utilises another Angular directive, [ng-submit](http://docs.angularjs.org/api/ng.directive:ngSubmit) which we’ll get to next. Return to your browser and the UI should now look similar to this:

![preview](img/preview-2.png)

@annotation:tour step3
#Making the Add button work
If you click the Add button currently, nothing will happen - let’s change that.

`ng-submit` binds an angular expression to the onsubmit event of the form. If no action attribute is applied to the form, it also prevents the default browser behaviour. In our example we’ve added an angular expression of ‘addTodo()’.

The following `addTodo` function pushes new todo items onto the existing todo items array and then clears the text input field. Let's add the following code inside the existing code

    $scope.addTodo = function () {
      $scope.todos.push($scope.todo);
      $scope.todo = '';
    };

So, your `app/scripts/controllers/main.js` should now look like this  ...

    'use strict';

    angular.module('workspaceApp')
      .controller('MainCtrl', function ($scope) {
        $scope.todos = ['Item 1', 'Item 2', 'Item 3'];
        $scope.addTodo = function () {
          $scope.todos.push($scope.todo);
          $scope.todo = '';
        };
      });

View the app in the browser again. Type some text in the input field for a new todo item and hit the Add button. It will be immediately reflected in your todos list. Here you can see I have added 2 new todos.

**Note**: if you enter in more than one blank todo item, or a todo item with the same name, your todo app will unexpectedly stop working. :( As a fun exercise on your own time, enhance the addTodo function with error checking.

![preview](img/preview-3.png)

@annotation:tour remove-todo
#Removing a ToDo
Let’s now add the ability to remove a todo item. We’ll need to add a new remove button alongside each todo item.

Going back to our view template (app/views/main.html), add a button to the existing `ng-repeat` directive. And to make sure our input field and remove button line up nicely, change the class on the paragraph tag from "form-group" to “input-group”.

Replace the stuff below `<!-- Todos list -->` with this code

    <!-- Todos list -->
    <p class="input-group" ng-repeat="todo in todos">
      <input type="text" ng-model="todo" class="form-control">
      <span class="input-group-btn">
        <button class="btn btn-danger" ng-click="removeTodo($index)" aria-label="Remove">X</button>
      </span>
    </p>

then run your app again, and you should get this ...

![remove](img/remove-buttons.png)

We introduced a new Angular directive above, [ng-click](http://docs.angularjs.org/api/ng.directive:ngClick). ng-click allows you to specify custom behaviours when an element is clicked. In this instance, we call `removeTodo()` and pass $index to the function.

The value of $index will be the array index of the current todo item within the ng-repeat directive. For example, the first item will have an array index of 0 and removeTodo will be passed the value of 0. Similarly, the last item of a todo list with 5 items will have an array index of 4 and `removeTodo` will be passed a value of 4.

@annotation:tour remove-todo-working
#Making the remove buttons work
Let’s now add some logic for removing todo items to our controller. The following `removeTodo` function removes one todo item from the items array using the JavaScript `splice` method at the given `$index` value:

    $scope.removeTodo = function (index) {
      $scope.todos.splice(index, 1);
    };

The complete controller (app/scripts/controllers/main.js) with the new `removeTodo` function is below:

'use strict';

    angular.module('mytodoApp')
      .controller('MainCtrl', function ($scope) {
        $scope.todos = ['Item 1', 'Item 2', 'Item 3'];
        $scope.addTodo = function () {
          $scope.todos.push($scope.todo);
          $scope.todo = '';
        };
        $scope.removeTodo = function (index) {
          $scope.todos.splice(index, 1);
        };
      });

Now you can use the 'x' buttons to remove items.

One thing you might notice is that although we’re able to add and remove items, we don’t have a way to persist them. Any time we refresh the page our todo items are reset back to the defaults in our todos array hardcoding in `main.js`. 

Don’t worry, we’ll fix this later after we learn more about installing packages with Bower.

@annotation:tour remove-todo-working







