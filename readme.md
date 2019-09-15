# StoutLabs Eslint and Prettier Setup

These are **my** current settings for ESLint and Prettier.

I totally based this off Wes Bos' [tutorial](https://www.youtube.com/watch?v=lHAeK8t94as) and setup repo, over at [eslint-config-wesbos](https://github.com/wesbos/eslint-config-wesbos)... but I have added several rules, tweaked some others, and updated all the dependencies to their current versions.

You should probably just use Wes' version... because it's **awesome**, and it is less opinionated than mine. (I just don't want to keep writing the same extra rules in an `.eslintrc` file over and over.) The instructions below are mostly his, but I've edited a few places and removed the mention of global installations. (Since that is no longer recommended as of eslint 6.0)

## Installing

As of eslint v6.0 it is now recommended to install eslint locally, once per project. (That way you can have project specific settings, and sync those settings with others working on your project via git.) Note: I also had trouble with a global installation on my Windows machine, using `nvm` running the current LTS version of node. YMMV, of course.

Anyhow, install locally like this:

1. If you don't already have a `package.json` file, create one with `npm init`.

2. Then we need to install everything needed by the config:

```
npx install-peerdeps --dev eslint-config-stoutlabs
```

3. You can see in your package.json there are now a big list of devDependencies.

4. Create a `.eslintrc` file in the root of your project's directory (it should live where package.json does). Your `.eslintrc` file should look like this:

```json
{
  "extends": ["stoutlabs"]
}
```

Tip: You can alternatively put this object in your `package.json` under the property `"eslintConfig":`. This makes one less file in your project.

5. You can add two scripts to your package.json to lint and/or fix:

```json
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint . --fix"
},
```

6. Now you can manually lint your code by running `npm run lint` and fix all fixable issues with `npm run lint:fix`. (You probably want your editor to do this, though. See below.)

---

## Config Settings

If you'd like to overwrite eslint or prettier settings, you can add the rules in your `.eslintrc` file.

The [ESLint rules](https://eslint.org/docs/rules/) go directly under `"rules"` while [prettier options](https://prettier.io/docs/en/options.html) go under `"prettier/prettier"`.

Note that prettier rules overwrite anything in my config (trailing comma, and single quote), so you'll need to include those as well.

```js
{
  "extends": [
    "stoutlabs"
  ],
  "rules": {
    "no-console": 2,
    "prettier/prettier": [
      "error",
      {
        "semi": false,
        "singleQuote": true,
        "tabWidth": 2
      }
    ]
  }
}
```

## Set Up With VS Code

Once you have done the above installs, you probably want your editor to lint and fix for you. (Trust me, you do.)

1. Install the [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) extension for VS Code
2. Update your VS Code settings via `Code/File` → `Preferences` → `Settings`. It's easier to enter these settings while editing the `settings.json` file, so click the `{}` icon (in the top right corner):

```js
"editor.formatOnSave": true,
// turn off for JS and JSX, we will do this via eslint
"[javascript]": {
  "editor.formatOnSave": false
},
"[javascriptreact]": {
  "editor.formatOnSave": false
},
// tell ESLint extension to run on save
"eslint.autoFixOnSave": true,

// Optional BUT IMPORTANT: If you have the prettier extension enabled for other languages like CSS and HTML, turn it off for JS since we are doing it through Eslint already
"prettier.disableLanguages": ["javascript", "javascriptreact"],
```

### Removal:

```sh
# or yarn, of course
npm remove --global eslint-config-stoutlabs babel-eslint eslint eslint-config-prettier eslint-config-airbnb eslint-plugin-html eslint-plugin-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react prettier eslint-plugin-react-hooks
```

To do the above for local, omit the `--global` flag.
