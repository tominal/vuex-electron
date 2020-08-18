<p align="center">
  <img width="750" src="https://user-images.githubusercontent.com/678665/45566726-404d9e80-b860-11e8-94b6-527dfcc3b3b3.png">
</p>

# Vuex Electron

[![Travis](https://img.shields.io/travis/com/vue-electron/vuex-electron.svg?style=for-the-badge&longCache=true)](https://travis-ci.com/vue-electron/vuex-electron)
[![Code Climate](https://img.shields.io/codeclimate/maintainability/vue-electron/vuex-electron.svg?style=for-the-badge&longCache=true)](https://codeclimate.com/github/vue-electron/vuex-electron)
[![Code Climate](https://img.shields.io/codeclimate/coverage/vue-electron/vuex-electron.svg?style=for-the-badge&longCache=true)](https://codeclimate.com/github/vue-electron/vuex-electron)
[![Code Style Prettier](https://img.shields.io/badge/code%20style-prettier-brightgreen.svg?style=for-the-badge&longCache=true)](https://github.com/prettier/prettier)
[![Made With Love](https://img.shields.io/badge/made%20with-love-brightgreen.svg?style=for-the-badge&longCache=true)](https://github.com/MrEmelianenko)

The easiest way to share your Vuex Store between all processes (including main).

### Features

:star: Persisted state  
:star: Shared mutations

### Requirements

- [Vue](https://github.com/vuejs/vue) v2.0+
- [Vuex](https://github.com/vuejs/vuex) v2.0+
- [Electron](https://github.com/electron/electron) v2.0+

### Installation

Installation of the Vuex Electron easy as 1-2-3.

1. Install package with using of [npm](https://github.com/npm/cli):

    ```
    npm install git+https://github.com/tominal/vuex-electron.git
    ```

2. Include plugins in your Vuex store::

    ```javascript
    import Vue from "vue"
    import Vuex from "vuex"

    import { createPersistedState, createSharedMutations } from "vuex-electron"

    Vue.use(Vuex)

    export default new Vuex.Store({
      // ...
      plugins: [
        createPersistedState({ throttle: 1000 }),
        createSharedMutations()
      ],
      // ...
    })
    ```

3. In case if you enabled `createSharedMutations()` plugin you need to create an instance of store in the main process. To do it just add this line into your main process (for example `src/main.js`):

    ```javascript
    import './path/to/your/store'
    ```

4. Well done you did it! The last step is to add the star to this repo :smile:

**Usage example: [Vuex Electron Example](https://github.com/vue-electron/vuex-electron-example)**

## IMPORTANT

In renderer process to call actions you need to use `dispatch` or `mapActions`. Don't use `commit` because actions fired via `commit` will not be shared between processes.

The ```throttle``` option was originally added by michaeljpeake and fixed by tominal. The issue arose from several random uncaught exceptions from the electron-vue repository. The persisted state is saved in several vuex-<timestamp>.json then renamed to vuex.json within the user's Electron app data folder. Windows apparently does not like several renames at once, so this is supposed to fix that.

### Options

Available options for `createPersistedState()`

```javascript
createPersistedState({
  storageName: 'vuex.json', // file name
  throttle: 0, // file rename throttle in milliseconds
  whitelist: ["whitelistedMutation", "anotherWhitelistedMutation"],

  // or

  whitelist: (mutation) => {
    return true
  },

  // or

  blacklist: ["ignoredMutation", "anotherIgnoredMutation"],

  // or

  blacklist: (mutation) => {
    return true
  }
})
```

### Author

Andrew Emelianenko  
IG: [@truemelianenko](https://www.instagram.com/truemelianenko)

michaeljpeake

tominal

### License

[MIT License](https://github.com/vue-electron/vuex-electron/blob/master/LICENSE)
