# Bootstrap for Sass: Starter Kit
This guide will help you start a new project working with Bootstrap 3 and Sass. This guide also assumes you are using macOS, although it should still be helpful for Windows/Linux users. However, there may be some additional dependencies or alternative methods for Windows or Linux. Inside the `project/pub/` directory, you'll find `index.html` which is an html version of this guide with some custom styling to show how one can use their own stylesheets without changing Bootstrap defaults. The `project` directory included with this guide is what your finished working directory should look like if you follow this guide. 

**Table of Contents:**
1. [Getting Started](#getting-started)
2. [Workflow Organization](#workflow-organization)
3. [Using Grunt to Compile Sass](#using-grunt-to-compile-sass)
4. [Conlusion](#conclusion)
5. [Useful Links](#useful-links)


## Getting Started
First, create a `project` folder; this is your main project directory that you'll keep all the files related to your project in. Inside it, create see a `pub` and `src` directory. These will be your *public* and *source* directories, respectively. Your public directory will contain just the files necessary for viewing the project in a browser, while the source directory will contain all the other files for building the project. It should look something like the template below:

    project
    |-- pub
    |-- src

***Note***: If any of the following installation commands do not work, try adding `sudo` to the beginning of the described command (e.g. `sudo gem install sass`). This runs the command with elevated privileges, which may be necessary for installing into your root user directory. It will also prompt you to enter your password.

### Installing Git
Download and install Git via [this link](gitlink), and follow the installer instructions.

You can check to ensure it's been installed if you run the following command in a Terminal session: 

    git --version
    
It should output something like `git version 2.10.1` if it is installed correctly.

*Learn more about git [here](#).*

### Installing Node.js
Download and install Node.js via [this link](#), and follow the installer instructions.

You can check to ensure it's been installed if you run the following command in a Terminal session: 

    npm -v
    
It should just output a version number such as `3.10.10` if it is installed correctly. Installing Node.js will allow you to use `npm`, the Node Package Manager.

*Learn more about Node.js & npm [here](#).*

### Installing Sass
Install SASS by running the following command in a Terminal session:

    gem install sass

***Note***: This will not work on Windows without having Ruby installed. macOS comes with Ruby installed by default, but on Windows machines you'll have to download Ruby using [this Ruby installer](http://rubyinstaller.org/).


### Installing Grunt (Global)
You'll be installing Grunt to help you compile your Sass files into a CSS file you'll use as your main stylesheet for your project (.scss -> .css). To install Grunt, run the following command in a Terminal session: 

    npm install -g grunt-cli
    
This installs grunt-cli globally to your root directory. You will install grunt locally into your project's source directory later in this guide. 

### Downloading Bootstrap
Finally, you should download Bootstrap from [Bootstrap's website](http://getbootstrap.com/getting-started/) (Click on the **Download Sass** button for the correct download). Once you've downloaded Bootstrap for Sass, unzip and move the contents of the folder into the `src` directory located inside the `project` folder.

***Note***: There are a few files that are normally hidden in Finder. I chose to move these files into the `src` folder as well, to keep things organized when using an IDE such as Brackets (Brackets shows hidden files in its sidebar navigation panel). To unhide the files in order to move them, I used [this short tutorial](http://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/). This may be prevented if you `npm install` inside `src` after you've installed

---

## Workflow Organization
Before you start setting up Grunt to help you compile your Sass files, you should reorganize the structure of your project now that you've downloaded Bootstrap. Organizing the project in the way I describe below makes following the rest of this guide and managing your project easier, but feel free to use naming conventions more appropriate to your preferences.

At this point you should have a few files and directories inside your `src` directory from Bootstrap, and the `npm install` dependencies. The one you should focus on first is the `assets` directory. Reorganize your project directory as follows: 

1. Inside the `assets` directory you should see `fonts`, `images`, `javascripts`, and `stylesheets`. Move only the `stylesheets`  directory out of `assets` and into the `src` directory (move it back one directory level). 

2. Now move the `assets` directory, which should on have `fonts`, `images`, and `javascripts` in it, into the `pub` directory. 

3. For simplicity, rename the `javascripts` and `stylesheets` directories to `js` and `sass` respectively. You should now have `assets` in your `pub` directory, and `sass` in your `src` directory.

4. Inside your `pub` directory, make a new folder called `css`. This is where your compiled css files will go (from .scss -> .css).

5. Now go inside your `sass` directory, and create a new file named `style.scss`. In the file type `@import "bootstrap";` on a single line and save the file. 

Now navigate to `project/src/` in Terminal (e.g. `cd <some_directory>/project/src`).
Run the following command in your `src` directory to install project dependencies:

    npm install
    
You should now see a new directory in `src` called `npm modules`.
    
---

## Using Grunt to Compile Sass
You've already installed Grunt globally, but now you'll be going through the process of getting it working in your project directory. First, in order to use Grunt you need a `package.json` in your project folder. However, you already have one created from the Bootstrap for Sass files you've moved into `src`. If you want to edit this package.json but aren't sure how, refer to the note at the bottom of this section.

### Installing Grunt (Local)
First, you need to install Grunt locally in your working project directory. In a Terminal session, navigate to 'project/src' and run the following command:

    npm install grunt --save-dev
    
This installs grunt to your project directory and adds it to the `devDependencies` in your `package.json` file. Next, you need to install two grunt plugins that will enable you to easily watch Sass files for changes and compile to CSS. In your Terminal session in`project/src` run the follwing commands: 
    
    npm install grunt-contrib-sass --save-dev
    npm install grunt-contrib-watch --save-dev

### Creating a Gruntfile.js
Now you need to create a `Gruntfile.js`. Create a new file called `Gruntfile.js` and copy the following code into it:

    module.exports = function(grunt) {
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
            sass: {
                dist: {
                    files: {
                        '../pub/css/style.css' : 'sass/style.scss'
                    }
                }
            },
            watch: {
                css: {
                    files: '**/*.scss',
                    tasks: ['sass']
                }
            }
        });
        grunt.loadNpmTasks('grunt-contrib-sass');
        grunt.loadNpmTasks('grunt-contrib-watch');
        grunt.registerTask('default', ['watch']);
    };
    
This `Gruntfile.js` assumes you've organized and named your project directory as described in the previous sections. Here's a brief breakdown of certain parts of the file to help you change it to better suit your personal project directory if necessary:

    sass: {
        dist: {
            files: {
                '../pub/css/style.css' : 'sass/style.scss'
            }
        }
    },

`sass: {...}` defines one of the Grunt tasks that will run when you're compiling Sass to CSS. `files: {...}` is where you'll actually define where your compiled .css file should go, and from what .scss file it should be compiled from. In the example you see `../pub/css/style.css' : 'sass/style.scss'`; this can be read as `'my_css_file_to_compile_to' : 'my_scss_file_to_compile_from'`.

    watch: {
        css: {
            files: '**/*.scss',
            tasks: ['sass']
        }
    }


`watch: {...}` defines another Grunt task which will run the task mentioned above, `sass: {...}`. The `watch` task can be used to run multiple different tasks that watch for changes in certain files and update other. Here `files: {...}` defines what files should be watched for changes. `tasks: {...}` defines what task should be run when a change is detected in the defined file(s).

    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');
    grunt.registerTask('default', ['watch']);
    
The first two lines are loading in the grunt plugins you installed in the last step, `grunt-contrib-sass` and `grunt-contrib-watch`. The last line defines what tasks should be run by default, when you run `grunt` without a specific command following it. This means you can just run `grunt` in your command line to watch your Sass files and compile them to CSS. You can inlcude additional tasks in the second argument of `grunt.registerTasks(...)` if you find yourself wanting to run multiple tasks frequently.


### Using Grunt Tasks

In this project, the Gruntfile.js is set up just for Sass compilation, which is all you need to get started with a Bootstrap for Sass project. The command you'll primarily be using is `grunt watch`. Using the previously described Gruntfile, `grunt watch` will be watching all .scss files in the `src/sass/` directory, and if it sees a change it will it will recompile the `style.css` file in `pub/css/`. This happens because in the `style.scss` file you created, the line `@import "bootstrap";`imports `bootstrap.scss`, which itself has imports of all the Bootstrap sass files.

So, to compile your Sass to CSS while you're working in your project directory:

1. Open up a Terminal session, and navigate to the `pub/src/` directory.
2. Run `grunt` or `grunt watch`.

---

## Conclusion
Now you're ready to start front-end development with Bootstrap for Sass!

Your working project directory should look something like this if you've followed this guide's organization instructions:

    project
    |-- pub
    |   |-- assets
    |   |   |-- fonts
    |   |   |-- images
    |   |-- css
    |   |   |-- style.css
    |   |   |-- style.css.map
    |   |-- js
    |   |   |-- bootstrap
    |   |   |-- bootstrap-sprockets.js
    |   |   |-- boostrap.js
    |   |   |-- boostrap.min.js
    |-- src
    |   |-- package.json
    |   |-- Gruntfile.js
    |   |-- sass
    |   |   |-- _bootstrap.scss
    |   |   |-- style.scss
    |   |   |-- ...
    |   |-- ...
   
   
   
---

## Useful Links

- A great IDE for web development: [Brackets](brackets.io) and recommended Brackets extensions:
    - [Brackets Markdown Preview](https://bitbucket.org/begue/brackets-markdown-preview/src) - Makes this guide easy to follow along while looking at your directory.
    - [Custom Work](https://github.com/DH3ALEJANDRO/custom-work-for-brackets) - Arguably a must have for Brackets users; creates tabs for working files, icons for directory sidebar.
    - [New Moon theme](https://github.com/taniarascia/new-moon) - A great theme for those who enjoy dark workspaces.
    
- Learn more about Grunt: [Getting started - Grunt: The Javascript Task Runner](https://gruntjs.com/getting-started)
- More on using Grunt & Sass together: [Getting Started with Grunt & Sass](https://www.taniarascia.com/getting-started-with-grunt-and-sass/)
- Learn about Sass: [Sass: Syntactically Awesome Style Sheets](http://sass-lang.com/)
- A good, brief explanation of Sass: [What's the difference between SCSS and Sass? - Stack Overflow](http://stackoverflow.com/questions/5654447/whats-the-difference-between-scss-and-sass)
