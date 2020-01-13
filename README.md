# LVUE

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://github.com/UI3es/LaravelVue/blob/master/README.md)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://github.com/UI3es/LaravelVue/blob/master/README.md)

LVUE is a cross-platform project providing access to paid templates on Vuetify.

  - Long Page
  - Templates blogs
  - Admin Pages

## About

This lesson provides an example of deploying such a project.

### Installation

LVUE recommends [Node.js](https://nodejs.org/) v12.0.0+ to run.

Install the dependencies and devDependencies and start the server.

> Before starting the deployment, it is assumed that you have already installed:

| Basic programs for work | LINKS |
| ------ | ------ |
| Open Server | https://ospanel.io |
| Node JS | https://nodejs.org |
| Sublime Text 3 | https://www.sublimetext.com |

| Back-end | Version | LINKS |
| ------ | ------ | ------ |
| PHP | ^7.1.3* | https://www.php.net |
| Laravel | ^5.8.* | https://laravel.com/docs/5.8 |
| MySQL | ^5.6* | https://www.mysql.com |

| Front-end | Version | LINKS | README |
| ------ | ------ | ------ | ------ |
| Vue JS | ^2* | https://vuejs.org | Javascript framework |
| Vue Router | ^3.* | https://vuejs.org/v2/guide/routing.html | Router |
| Vue Resource | ^1.5.* | https://github.com/pagekit/vue-resource | For ajax-inquiry and promise |
| Vuetify | ^2.* | https://vuetifyjs.com/ru/getting-started/quick-start | UI framework |
| Sass | ^1.15.* | https://sass-lang.com | CSS based metalanguage |
| Webpack | ^3 | https://webpack.js.org | JavaScript module package |


#### Step 1

Laravel Deployment

```sh
$ cd domains
$ composer global require laravel/installer
$ composer create-project --prefer-dist laravel/laravel="5.8.*" LVUE

or
$ composer create-project --prefer-dist laravel/laravel LVUE "5.8.*"

and or
$ composer create-project --prefer-dist laravel/laravel LVUE
```

##### Step 1.1

> (*Optional*) Remove preset scaffolding

This is the optional step. We are not using the scaffolding in this tutorial. Therefore, I am going to remove it to keep the folder structure as neat as possible. Simply enter the command below.

```sh
$ cd LVUE
$ php artisan preset none
```

#### Step 2

> Initially, in Laravel, the starting point for the interface is the “resource” folder. We will create the project in a separate folder “front” and redirect the render to it.

##### Step 2.1

Open the webpack.mix.js file and edit it.

```javascript
const mix = require('laravel-mix');

/*
mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
*/

if (mix.inProduction()) {
    console.log("\x1b[41m\x1b[1m%s\x1b[0m", 'Assembly in progress...');
    mix.autoload({})
        .js('front/main.js', 'public/js')
        .sass('front/assets/sass/app.scss', 'public/css')
        .webpackConfig({
            resolve: {
                alias: {
                    RootDir: path.resolve(__dirname, 'front/'),
                    PublicDir: path.resolve(__dirname, 'public/')
                }
            }
        })
        .version();
} else {
    mix.autoload({})
        .js('front/main.js', 'public/js')
        .sass('front/assets/sass/app.scss', 'public/css')
        .webpackConfig({
            resolve: {
                alias: {
                    RootDir: path.resolve(__dirname, 'front/'),
                    PublicDir: path.resolve(__dirname, 'public/')
                }
            }
        });
}
```

##### Step 2.2

Create Vue Project

> You can use the standard command:
> ```sh
> $ vue create front
> ```
> (*optional*) or use this command if you want directly through the package `npm`:
> ```sh
> $ vue create front --packageManager npm
> ```
> > Sometimes the console does not accept `vue`, this tells us that you do not have `vue`. Use the following command:
> > ```sh
> > $ npm install -g @vue/cli
> > ```
> > After check the version, if the version index is shown - you installed `vue`, you can continue:
> > ```sh
> > $ vue --version
> > ```


###### Step 2.2.1

After starting the team, we will be given a choice of presets. Choose like me:

```sh
> Manually select features
```

The default settings are great for quickly prototyping a new project, while manual setting provides more options that may be required.

| Package | README |
| ------ | ------ |
| (*Default*) Babel | Excludes files inside `node_modules` dependencies |
| Router | Routing |
| Vuex | Used for data storage |
| (*Default*) Linter / Formatter | Allows tracking errors |

```sh
>(*) Babel
>(*) Router
>() Vuex //I did not choose Vuex, because we will use Laravel, but you can choose Vuex if you do not have a Back-end.
>(*) Linter / Formatter
```

In the next window we are asked whether to use the history mode in the router. I just hit Enter to select the default value of Yes.

```sh
>(*) Use history mode for router? (Y/n) Y
```

In the next window, we need to choose how we want to configure Linter. I chose ESLint with error prevention only.

```sh
>(*) ESLint with error prevention only
```

In the next window, we select when Linter is used to verify the project. I chose Lint to save, which means that the check applies every time a file is used.

```sh
>(*) Lint to save
```

In the next window, we must decide how we want to customize our project. Select all configuration options in the package.json file. I prefer to store everything in one file, so I choose the option with package.json.

```sh
>(*) In package.json
```

In the last window, we have the opportunity to save the entire configuration as an easy-to-use preset for creating future projects. I typed "N". Presets are stored in the user directory inside the .vuerc file.

```sh
> Save this as a preset for future projects? (y/N) N
```

###### Step 2.2.2

Add Vuetify

```sh
$ cd front
$ vue add vuetify
```

> Perhaps you will have such a message (`WARN There are uncommited changes in the current repository, it's recommended to commit or stash them first. Still proceed? Y`)

You will be prompted to select a preset, select `Default`

```sh
> Choose a preset: Default (recommended)
```

#### Step 3

Editing project files before starting.

> The main difficulty arises when you run the `npm run watch` command (from the root folder of the project), the console will throw an error, because earlier we redirected the entire front-end to the` front` folder. If you look there, we will find nesting in the form of the `src` folder, we need to get rid of the nesting.

##### Step 3.1

At the very beginning, you need to copy the json file parameters from `/ front / package.json` to` / package.json`. The final result for January 8, 2020 is presented below.

```json
    //ADD
    "dependencies": {
        "core-js": "^3.4.4",
        "vue": "^2.6.10",
        "vue-router": "^3.1.3",
        "vuetify": "^2.1.0"
    },
    "devDependencies": {
        //ADD
        "@vue/cli-plugin-babel": "^4.1.0",
        "@vue/cli-plugin-eslint": "^4.1.0",
        "@vue/cli-plugin-router": "^4.1.0",
        "@vue/cli-service": "^4.1.0",
        //WAS
        "axios": "^0.19",
        "bootstrap": "^4.1.0",
        "cross-env": "^5.1",
        "jquery": "^3.2",
        "laravel-mix": "^4.0.7",
        "lodash": "^4.17.13",
        "popper.js": "^1.12",
        "resolve-url-loader": "^2.3.1",
        "vue": "^2.5.17",
        //ADD
        "babel-eslint": "^10.0.3",
        "eslint": "^5.16.0",
        "eslint-plugin-vue": "^5.0.0",
        "sass": "^1.15.2",
        "sass-loader": "^7.1.0",
        "vue-cli-plugin-vuetify": "^2.0.3",
        "vue-template-compiler": "^2.6.10",
        "vuetify-loader": "^1.3.0"
    }
}
```

##### Step 3.2

Transfer the necessary vue-cli and vuetify configuration files to the project root.

| From | To |
| ------ | ------ |
| /front/vue.config.js | /vue.config.js |
| /front/babel.config.js | /babel.config.js |
| /front/src | /src/ |

By clearing the `front` folder and transferring the files from` src` to `front`, you can delete the` src` folder.

| From | To |
| ------ | ------ |
| /src/* | /front/* |

##### Step 3.3

Now you need to register the following command in the console:

```sh
$ npm install
```

#### Step 4

That is not all, you need to edit several files.

##### Step 4.1

After successfully installing all the modules using *npm*, go to the address `/resources/views/` and create a file called `front.blade.php`. Enter the following code there:

```html
<!DOCTYPE HTML>
    <html lang="ru">
    <head>
        <meta charset="utf-8">
        <meta name="mobile-web-app-capable" content="yes">
        <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, minimal-ui">
        <meta name="csrf-token" content="{{ csrf_token() }}">
        <title>LVUE</title>
        <link href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900" rel="stylesheet">
        <link href="https://cdn.jsdelivr.net/npm/@mdi/font@4.x/css/materialdesignicons.min.css" rel="stylesheet">
        <link rel="stylesheet" href="{{ mix('css/app.css') }}">
    </head>
    <body>
        <div id="app"></div>
    </body>
    <script src="{{ mix('js/main.js') }}"></script>
</html>
```

##### Step 4.2

Go to address `/front/plugins/` open file `vuetify.js` and change the line. Also do not forget to add a line of css styles:

| From | To |
| ------ | ------ |
| `import Vuetify from 'vuetify/lib';` | `import Vuetify from 'vuetify';` |
|  | `import 'vuetify/dist/vuetify.min.css';` |

##### Step 4.3

Create file `app.scss` by address `/front/assets/sass/app.scss`

##### Step 4.4

Open file `/routes/web.php` and replace `welcome` to `front`.

##### Step 4.5

After all the manipulations, comment out all the lines associated with `HelloWorld` in the *Home.vue* and *App.vue* files

> After looking at the console, there should be no errors.

- App.vue
```html
<template>
	<v-app>
		<router-view></router-view>
	</v-app>
</template>

<script>
	// import HelloWorld from './components/HelloWorld';

export default {
	name: 'App',

	components: {
		// HelloWorld,
	},

	data: () => ({
		//
	}),
};
</script>
```

- Home.vue
```html
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    // HelloWorld
  }
};
</script>
```

#### Step 5

It remains to create the initial configuration of the application. We will use the official site [Vuetify](https://vuetifyjs.com/ru/components/application).

```html
<template>
	<v-app>
		<v-navigation-drawer
		v-model="drawer"
		app
		clipped
		>
			<v-list dense>
				<v-list-item link to="/">
					<v-list-item-action>
						<v-icon>mdi-view-dashboard</v-icon>
					</v-list-item-action>
					<v-list-item-content>
						<v-list-item-title>Dashboard</v-list-item-title>
					</v-list-item-content>
				</v-list-item>
				<v-list-item link to="/about">
					<v-list-item-action>
						<v-icon>mdi-settings</v-icon>
					</v-list-item-action>
					<v-list-item-content>
						<v-list-item-title>Settings</v-list-item-title>
					</v-list-item-content>
				</v-list-item>
			</v-list>
		</v-navigation-drawer>

		<v-app-bar
		app
		clipped-left
		>
			<v-app-bar-nav-icon @click.stop="drawer = !drawer" />
			<v-toolbar-title>Application</v-toolbar-title>
		</v-app-bar>

		<v-content>
			<v-container fluid>
				<router-view></router-view>
			</v-container>
		</v-content>

		<v-footer app>
			<span>&copy; 2k20</span>
		</v-footer>
	</v-app>
</template>

<script>
	export default {
		name: 'App',

		components: {
			//
		},

		props: {
	      source: String,
	    },

	    data: () => ({
	      drawer: null,
	    }),
	};
</script>
```

You see:

![](https://github.com/UI3es/Project_images/blob/master/LVUE/App.jpg)
