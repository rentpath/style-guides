# Style Guides
A collection of style guides used at RentPath.

## Lint config files
### SASS
  Uses node's sass-lint. Copy `css/.sass-lint.yml` to your project directory and change the src option to a glob that includes your `.scss` files. To run it, install sass-lint (`npm install sass-lint`) and run `sass-lint -v`. If there are any errors or warnings, they should show in your output. 
  
  To run with webpack, You'll need to add the configfile to your webpack file.
   
  ```javascript
  module.exports = {
    sasslint: {
      configFlie: '.sass-lint.yml'
    }
  }
  
  ``` 
  and then add it to your preloaders
  
  ```javascript
  module: {
    preLoaders: [
      {
        test: /\.scss?$/,
        exclude: [/node_modules/],
        loaders: ['sasslint'],
        //this should change with your project and point to where your scss files are
        include: path.join(__dirname, '../src')
      }
    ]
  }
  ```
  
## Editor Setup

- [Atom](https://github.com/rentpath/style-guides/wiki/Setup-Atom-Linter)

- [Vim](https://github.com/rentpath/style-guides/wiki/Vim-Linter)

- [Sublime](https://github.com/rentpath/style-guides/wiki/Sublime-Linter)

![x](http://i.imgur.com/X60PlOu.png)
