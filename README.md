## Ryujin Jakka - Front-end
This is the front-end of the project *Ryujin Jakka* which is an web app made for Anime.

### Configurations
### Create the React app *Ruyjin Jakka* by using,
- Create the React app using the latest version.

    `yarn create react-app ryujin-jakka --template typescript`
- Delete *node_modules* folder and *yarn.lock*.
- Replace the *package.json* with the following.

    ```
    {
        "name": "ryujin-jakka",
        "version": "0.1.0",
        "private": true,
        "scripts": {
            "dev": "vite",
            "build": "tsc && vite build",
            "serve": "vite preview"
        },
        "dependencies": {
            "axios": "^0.21.1",
            "react": "^17.0.0",
            "react-dom": "^17.0.0",
            "web-vitals":"^1.0.1"
        },
        "devDependencies": {
            "@types/react": "^17.0.0",
            "@types/react-dom": "^17.0.0",
            "@vitejs/plugin-react-refresh": "^1.3.1",
            "typescript": "^4.3.2",
            "vite": "^2.3.7"
        }
    }
    ```
- Install the dependencies with `yarn install`.
- Here, vite can be seen in the scripts. Vite is a modern build tool that provides a blazing fast development experience and can be used instead of CRA. But here, we will be using CRA and use the above .json file only to scaffold our project.

### Add Craco

- The Create React App CLI doesn't provide a way of overrideing its internal configurations.
- Therefore, we need to first install a tool that can help us with that.
- Two very popular ones are *Craco* and *react-app-rewired*.
- We will be using the former one.
- Run the following command to install craco. 
    `yarn add @craco/craco@^6.2.0`

### Update the *scripts* section in the *package.json* file.
```
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test:unit": "craco test"
}
```

### Create a config file named *craco.config.js* in the root directory.
```
module.exports = {}
```

### PostCSS
- Install dependencies.

    `yarn add stylelint@13.13.1 postcss@^7.0.36 postcss-import@12.0.1 postcss-extend@1.0.5 postcss-nested@4.2.3 postcss-preset-env@6.7.0 postcss-reporter@6.0.1 precss@4.0.0 --dev`
- Create *postcss.config.js* file.
    
    ```
    module.exports = {
        plugins: [
            require('stylelint')({
                configFile: 'stylelint.config.js',
            }),
            require('postcss-extend'),
            require('precss'),
            require('postcss-preset-env'),
            // uncomment if you're using Tailwind
            // require('tailwindcss')('taiwind.config.js'),
            require('postcss-nested'),
            require('autoprefixer')(),
            require('postcss-reporter'),
        ],
    }
    ```
- Update *craco.config.js* to make use of the PostCSS config
   
    ```
    const postcssConfig = require('./postcss.config')
    module.exports = {
        style: {
            postcss: postcssConfig,
        },
    }
    ```

### Stylelint
- Install dependencies.
    
    `yarn add stylelint-config-css-modules@^2.2.0 stylelint-config-prettier@^8.0.2 stylelint-config-recess-order@^2.4.0 stylelint-config-recommended@^5.0.0 stylelint-config-standard@^22.0.0 stylelint-scss@^3.19.0 --dev`
- Create *stylelint.config.js* file.

    ```
    module.exports = {
        extends: [
            'stylelint-config-recommended',
            'stylelint-config-standard',
            'stylelint-config-recess-order',
            'stylelint-config-css-modules',
            'stylelint-config-prettier',
        ],
        plugins: ['stylelint-scss'],
        ignoreFiles: [
            './coverage/**/*.css',
            './dist/**/*.css',
            './node_modules/**/*.css',
        ],
        rules: {
            'at-rule-no-unknown': [
            true,
            {
                ignoreAtRules: [
                // --------
                // Tailwind
                // --------
                'tailwind',
                'apply',
                'variants',
                'responsive',
                'screen',
                ],
            },
            ],
            'declaration-block-no-duplicate-custom-properties': null,
            'named-grid-areas-no-invalid': null,
            'no-duplicate-selectors': null,
            'no-empty-source': null,
            'selector-pseudo-element-no-unknown': null,
            'declaration-block-trailing-semicolon': null,
            'no-descending-specificity': null,
            'string-no-newline': null,
            // Use camelCase for classes and ids only. Works better with CSS modules
            // 'selector-class-pattern': /^[a-z][a-zA-Z]*(-(enter|leave)(-(active|to))?)?$/,
            // 'selector-id-pattern': /^[a-z][a-zA-Z]*$/,
            // Limit the number of universal selectors in a selector,
            // to avoid very slow selectors
            'selector-max-universal': 1,
            // --------
            // SCSS rules
            // --------
            'scss/dollar-variable-colon-space-before': 'never',
            'scss/dollar-variable-colon-space-after': 'always',
            'scss/dollar-variable-no-missing-interpolation': true,
            'scss/dollar-variable-pattern': /^[a-z-]+$/,
            'scss/double-slash-comment-whitespace-inside': 'always',
            'scss/operator-no-newline-before': true,
            'scss/operator-no-unspaced': true,
            'scss/selector-no-redundant-nesting-selector': true,
            // Allow SCSS and CSS module keywords beginning with `@`
            'scss/at-rule-no-unknown': null,
        },
    }
    ```
### Tailwind
- Install the dependencies.

    `yarn add tailwindcss@npm:@tailwindcss/postcss7-compat@^2.2.4 autoprefixer@^9.8.6 --dev`

    `yarn add @tailwindcss/forms@^0.3.3`

- Create a tailwind config file.

    `npx tailwindcss init -p`

- Modify *tailwind.config.js*.

    ```
    const colors = require('tailwindcss/colors')

    module.exports = {
        purge: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
        mode: 'jit',
        darkMode: false, // or 'media' or 'class'
        theme: {
            extend: {
            fontFamily: {
                heading: ['Montserrat', 'sans-serif'],
                content: ['Nunito', 'sans-serif'],
            },
            },
            colors: {
            ...colors,
            },
        },
        variants: {
            extend: {},
        },
        plugins: [require('@tailwindcss/forms')],
    }
    ```
- Update *src/index.css* to include Tailwind directives.
    
    ```
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

### Prettier
- Install prettier.

    `yarn add prettier@^2.3.2 --dev`
- Configure the prettier according to your preferences.

    ```
    module.exports = {
        endOfLine: 'lf',
        jsxBracketSameLine: false,
        printWidth: 80,
        proseWrap: 'never',
        quoteProps: 'as-needed',
        semi: false,
        singleQuote: true,
        jsxSingleQuote: false,
        tabWidth: 2,
        trailingComma: 'es5',
        useTabs: false,
        vueIndentScriptAndStyle: false,
    };
    ```

### Typescript
- Install typesript

    `yarn add typescript@^4.3.2 --dev`
- When scaffolding the project CRA automatically creates *tsconfig.json* file with some defaults and strict mode enabled.
- Since we need to have unconfusing relative paths we can use *craco-alias* for that.

    `yarn add craco-alias@^3.0.1 --dev`
- Modify the *craco.config.js* file.

    ```
    const postcssConfig = require('./postcss.config')
    const cracoAlias = require('craco-alias')
    module.exports = {
        style: {
            postcss: postcssConfig,
        },
        plugins: [
            {
            plugin: cracoAlias,
            options: {
                source: 'tsconfig',
                // baseUrl SHOULD be specified
                // plugin does not take it from tsconfig
                baseUrl: './',
                /* tsConfigPath should point to the file where "baseUrl" and "paths" 
                are specified*/
                tsConfigPath: './tsconfig.paths.json',
            },
            },
        ],
    }
    ```
- We need to specify `source`, `baseUrl` and `tsConfigPath` properties. To do that we will create a *tsconfig.json* file.

    ```
    {
        "compilerOptions": {
            "paths": {
            "@/*": ["src/*"]
            }
        }
    }
    ```
- This setup will resolve imports starting with `@` sign to the `src` directory. If necessary, you can add more paths for resolving components, assets, etc.

    ```
    // ugly
    import Component from '../../../components/common/MyComponent'

    // nice
    import Component from '@/components/common/MyComponent' 
    ```
- Update the *tsconfig.json* file.
    
    ```
    {
        "extends": "./tsconfig.paths.json",
        "compilerOptions": {
            "baseUrl": ".",
            "target": "es5",
            "lib": [
            "DOM",
            "DOM.Iterable",
            "ESNext"
            ],
            "allowJs": true,
            "skipLibCheck": true,
            "esModuleInterop": true,
            "allowSyntheticDefaultImports": true,
            "strict": true,
            "forceConsistentCasingInFileNames": true,
            "noFallthroughCasesInSwitch": true,
            "module": "esnext",
            "moduleResolution": "node",
            "resolveJsonModule": true,
            "isolatedModules": true,
            "noEmit": true,
            "jsx": "react-jsx"
        },
        "include": [
            "src"
        ]
    }
    ```

### Jest, Cypress, and Testing Library
- Install cypress.

    `yarn add cypress@^8.2.0 @testing-library/cypress@^7.0.6 --dev`

- Add scripts to *package.json*.

    ```
    "scripts": {
        "start": "craco start",
        "build": "craco build",
        "test:unit": "craco test",
        "test:e2e:open": "start-server-and-test start http-get://localhost:3000 cypress:open",
        "test:e2e:run": "start-server-and-test start http-get://localhost:3000 cypress:run",
        "cypress:run": "cypress run",
        "cypress:open": "cypress open"
    }
    ```
- When you run Cypress for the first time, it would automatically create a new directory called *cypress* at the root of the project. To run first time type `yarn run cypress:open` in terminal.
- It contains,
    - fixtures
    - integration
    - plugins
    - support
- We can remove all the example tests from *cypress/integration* folder, since we won't need them.
- Update the `.js` files to `.ts`.
- *cypress/plugins/index.ts*

    ```
    /// <reference types="cypress" />
    /* eslint-disable import/no-anonymous-default-export */
    /**
    * @type {Cypress.PluginConfig}
    */
    export default (
    on: Cypress.PluginEvents,
    config: Cypress.PluginConfigOptions
    ) => {
    return Object.assign({}, config, {
        fixturesFolder: 'cypress/fixtures',
        integrationFolder: 'cypress/integration',
        screenshotsFolder: 'cypress/screenshots',
        videosFolder: 'cypress/videos',
        supportFile: 'cypress/support/index.ts',
    })
    }
    ```
- *cypress/support/commands.ts*

    ```
    // ***********************************************
    // This example commands.js shows you how to
    // create various custom commands and overwrite
    // existing commands.
    //
    // For more comprehensive examples of custom
    // commands please read more here:
    // https://on.cypress.io/custom-commands
    // ***********************************************
    //
    //
    // -- This is a parent command --
    // Cypress.Commands.add("login", (email, password) => { ... })
    //
    //
    // -- This is a child command --
    // Cypress.Commands.add("drag", { prevSubject: 'element'}, (subject, options) => { ... })
    //
    //
    // -- This is a dual command --
    // Cypress.Commands.add("dismiss", { prevSubject: 'optional'}, (subject, options) => { ... })
    //
    //
    // -- This is will overwrite an existing command --
    // Cypress.Commands.overwrite("visit", (originalFn, url, options) => { ... })
    import '@testing-library/cypress/add-commands'
    ```
- *cypress/support/index.ts*

    ```
    // ***********************************************************
    // This example support/index.js is processed and
    // loaded automatically before your test files.
    //
    // This is a great place to put global configuration and
    // behavior that modifies Cypress.
    //
    // You can change the location of this file or turn off
    // automatically serving support files with the
    // 'supportFile' configuration option.
    //
    // You can read more here:
    // https://on.cypress.io/configuration
    // ***********************************************************

    // Import commands.js using ES2015 syntax:
    import './commands'

    // Alternatively you can use CommonJS syntax:
    // require('./commands')
    ```
- After that create a *cypress.json*.

    ```
    {
        "baseUrl": "http://localhost:3000",
        "pluginsFile": "cypress/plugins/index.ts"
    }
    ```
- Add the following to the *.gitignore* file.

    ```
    /cypress/videos
    /cypress/screenshots
    ```
- Create a *cypress/tsconfig.json*.

    ```
    {
        "extends": "../tsconfig.json",
        "compilerOptions": {
            "noEmit": true,
            "types": ["cypress", "@testing-library/cypress"],
            "isolatedModules": false
        },
        "include": [
            "../node_modules/cypress",
            "../node_modules/@testing-library/cypress",
            "./**/*.ts"
        ]
    }
    ```
- Install Jest & Testing libraries

    `yarn add @testing-library/react@^11.1.0 @testing-library/jest-dom@^5.11.4 @testing-library/react-hooks@^7.0.1 @testing-library/user-event@^12.1.10 --dev`

- Change *src.setupTests.ts*

    `import @testing-library/jest-dom`

### Formatting Code Automatically on Commit
- Since we have already installed the prettier, we will install husky and lint-staged.

    `yarn add husky@^7.0.1 lint-staged@^11.1.2 --dev`

- Update the *package.json*

    ```
    "husky": {
        "hooks": {
        "pre-commit": "lint-staged"
        }
    },
    "lint-staged": {
        "*.{ts,tsx}": "eslint",
        "*.{css,scss}": "stylelint",
        "**/*.{js,jsx,ts,tsx,json,css,scss,md}": "prettier -w -u"
    }
    ```

- And add the following default configs as well.

    ```
    "eslintConfig": {
        "extends": [
        "react-app",
        "react-app/jest"
        ]
    },
    "browserslist": {
        "production": [
        ">0.2%",
        "not dead",
        "not op_mini all"
        ],
        "development": [
        "last 1 chrome version",
        "last 1 firefox version",
        "last 1 safari version"
        ]
    }
    ```

### Remove unwanted dependencies
- Remove @vitejs/plugin-react-refresh by

    `yarn remove @vitejs/plugin-react-refresh`

## Alternative Options for CRA
- The best option for CRA is to use Vite. You can find more details about it at,
vitejs.dev

## VS Code Extensions
The following VS Code extenstions can be used to improve code maintainance and consistency.
- ES7 React/Redux/GraphQL/React-Native snippets
- VSCode React Refactor
- Prettier
- ESLint
- Stylelint
- GitLens
- Git history
- Settings Sync
- Bracket Pair Colorizer 2
- Auto Close Tag
- Auto Rename
- Auto Import
- Import Cost
- Jumpy
- ES6 Snippets
- i18n Ally
- Formatting toggle
- npm intellisense
- Web Accessibility
- Live Share
- Better comments
- Markdownlint
- Docker
- Remote - SSH
- Remot - WSL
- Live Server
- Debugger for Chrome
- change0case
- Regex Previewer
- DotENV
- Inline Parameters for VSCode

If you are not using CRA, ESlint and Stylelint rules may have conflicts with Prettier. To avoid that, you can use, eslint-config-prettier and stylelint-config-prettier.