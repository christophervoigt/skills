# skills

Ok, so this is a repository for AI skills that I want to be able to install and update wherever I want.
To do this I'll use [openskills](https://github.com/numman-ali/openskills), which seems like a good tool to manage skills more centrally and without the need for copy-pasting.

*Note:*
_I filled this repository while I was on holiday in Bergen, Norway. I did not bring a laptop with me so I used my phone to write this. Imagine yourself doing this for a second: No keyboard. No fancy IDE. No terminal. Just the GitHub app on your phone (and a Dream). This is a wild ride!_😅

## What skills you ask?

Let's start with something simple like code style.

### setup-prettier

So [prettier](https://github.com/prettier/prettier) is a code formatter and I basically use the same config in all my projects all the time. It contains rules for file formatting that I grew accustomed to and I don't want to go through the hassle of setting it up all the time by myself anymore.

What the skill should do:

1. If a `package.json` exists and prettier is not already installed, install `prettier`.
   * If `astro` is in the dependency list, also install `prettier-plugin-astro`
1. Copy the `prettier.config.mjs` from the skill folder and add it to the root of the project.
   * If `astro` is in the dependency list, add `plugins: ['prettier-plugin-astro'],` at the start of config object.
1. Create an empty `.prettierignore` file. (This is mostly depending on the project.)
1. Add a `"prettier": "prettier . --write"` script to the package.json.
1. If a `.vscode` folder exists and it contains a `settings.json` add `"prettier.configPath": "prettier.config.mjs"` at the end of the settings.
