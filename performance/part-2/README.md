# Code splitting

We know how to bundle javascript files into one single file. But as it gets bigger and bigger, the load time
grows significantly too.

So why ship javascript if a user won't need it until he or she visits a second page ? 

Thus, we need **code splitting** : As more resources arrive the app can lazy load and unlock more features. Lazy loading means just loading them after your page becomes interactive so that the user can feel like the app is fast.

<img src="code_splitting.png" width="500px"/>

By using import we automatically tell Webpack to code split.

We're simply just removing the imports from the top and importing them only when we need them when the user clicks on a roue.

### Resources

- https://reactjs.org/docs/code-splitting.html
- https://github.com/jamiebuilds/react-loadable
- https://webpack.js.org/guides/code-splitting/