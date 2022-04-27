
### Create the React app *Ruyjin Jakka* by using,

`yarn create react-app ryujin-jakka --template typescript`

### Add Craco

- The Create React App CLI doesn't provide a way of overrideing its internal configurations.
- Therefore, we need to first install a tool that can help us with that.
- Two very popular ones are *Craco* and *react-app-rewired*.
- We will be using the former one.
- Run the following command to install craco. 
    - `yarn add @craco/craco`

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
- Install dependencies
    - `yarn add -D stylelint@13.13.1 postcss-import@12.0.1 postcss-extend@1.0.5 postcss-nested@4.2.3 postcss-preset-env@6.7.0 postcss-reporter@6.0.1 precss@4.0.0`
- Create *postcss.config.js* file
    
    - 
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
   
    - 
    ```
    const postcssConfig = require('./postcss.config')
    module.exports = {
        style: {
            postcss: postcssConfig,
        },
    }
    ```

### Stylelint
- Install dependencies
     - `yarn add -D stylelint-config-css-modules stylelint-config-prettier stylelint-config-recess-order stylelint-config-standard stylelint-scss`
- Create *stylelint.config.js* file

    - 
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
