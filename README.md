# Solid and GitHub Pages

A demonstration project to deploy a Solid.js application to GitHub Pages.

## Tooling
- [Solid.js](https://www.solidjs.com/)
- [GitHub Pages Service](https://pages.github.com/)
- [gh-pages module](https://www.npmjs.com/package/gh-pages)
- [Vite](https://vitejs.dev/)
- [pnpm](https://pnpm.io/)
- [degit](https://www.npmjs.com/package/degit)

## Setup
A note on package management:  
These setup instructions use 'pnpm' as the package manager. However, it can substituted with the equivalent `npm` or `yarn` command.  
ex) replace `pnpm install ...` with `npm install ...` or `yarn install ...`

### Initialize the application

Starting in the terminal of your local machine, initialize a Solid application using the [solid template](https://www.solidjs.com/guides/getting-started#try-solid). There are templates for javascript and typescript. We will use the typescript template. However, either template will work.

Install degit globally, if it doesn't exist on your machine.
```
pnpm install -g degit
```  

Initialize the Solid application 
```
npx degit solidjs/templates/ts my-app
cd my-app
pnpm install
```

### Initialize git
We will need a `git` history to interface with `GitHub` and create a branch specific to deployment. A `git` history is not created automatically. We will need to start it manually.

```
git init
```

This should also generate a `.gitignore` with `dist` and `node-modules`. We will want to exclude these folders from the git history.
If this `.gitignore` does not exist, create it now. It should match the [this repository's .gitignore](/.gitignore).

### Create GitHub repository
Each GitHub Page is built from a GitHub repository. We will need to create one to host the website. 
1) Select "New" on the repositories page.
2) Name the repository.
3) Create repository. (Skip adding any template, readme, license, or .gitignore. These will come from local repository)
4) Copy the link from the "Quick setup" on the next page. This is the link to the GitHub repository. It should be the url to the repository, followed by a `.git` extension.

### Link to GitHub
Add the GitHub repository as remote resource for the local project. We will title it `origin`. This is the traditional naming pattern.
It is also the name that our site will look for later, when we are deploying the application.
Back in the `my-app` folder of your computer's terminal, run:  

```
git remote add origin [link to GitHub repository]
```

### Configure gh-pages package
When deploying the site, GitHub pages will look for a production build of the application on the `gh-pages` branch of the repository.
This site is configured to store its production build in the `dist` folder. If you recall from earlier, we tell git to ignore this folder.
This means we want to manage our `gh-pages` branch differently than our development branches. The `gh-pages` module helps us mange this special branch.  
To configure the module:

1) Install it as development requirement
   - `pnpm install --save-dev gh-pages`
2) Create a script for deployment.
   - open the [package.json](/package.json) file
   - under `scripts`, add `"deploy": "gh-pages -d dist",`

### Configure Vite
When building the application, Vite will assume that it is hosted on a root path. However, the GitHub pages route will include the name of repository.
We will need to tell Vite about this path. This is done with the `base` option of the [Vite config](https://vitejs.dev/config/#base).

- add `base: '/[your-repo-name]/',` to the [vite.config.ts](/vite.config.ts)

Note: If you need to specify resource paths throughout your application relative to this path,
it is available through [`import.meta.env.BASE_URL`](https://vitejs.dev/guide/env-and-mode.html).

### Deploy
We are finally ready to run our first deployment.

1) Create a production build of the application
  - `pnpm run build`
2) Deploy the application to GitHub pages
  - `pnpm run deploy`

### Visit your site
GitHub Pages follows a format of `[account-name].github.io/[repository-name]`.
You can visit your site there. 

### Save your code to GitHub
The `gh-pages` module's deploy script will create a `gh-pages` branch in your repository that contains only the production build. You can create other branches to track your source code.

```
git add .
git commit -m '[message]'
git push origin [branch-name]
```

## Thank You
Thank you, gitname. The [react-gh-pages example](https://github.com/gitname/react-gh-pages)
served as a guideline for this Solid.js version.

