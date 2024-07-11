# (My fork only) Deprecated and replaced: https://github.com/zolacti/on


# Zola Deploy Action

A GitHub action to automatically build and deploy your [zola] site to the master
branch as GitHub Pages.

## Table of Contents

 - [Usage](#usage)
 - [Input Variables](#input-variables)
 - [Custom Domain](#custom-domain)

## Usage

In your repository **Settings > Actions > General**, in Workflow permissions, make sure that `GITHUB_TOKEN` has **Read and Write permissions**.

Depends on the default GitHub Action for GH Pages being set to deploy on on push to the target branch (gh-pages) 

Check the .github/workflows/test_build.yml for the full set of tests as examples.

This example will build and push to gh-pages on push to the main branch. The default GH Pages action will then deploy if set.

```
name: Zola on GitHub Pages

on: 
 push:
  branches:
   - main
    
jobs:        
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: shalzz/zola-deploy-action@master 
```

This example will build and deploy to gh-pages branch on a push to the main branch (while passing some more inputs)
and it will build only on pull requests.
```
name: Zola on GitHub Pages

on:
  push:
    branches:
      - main 
  pull_request:
  
jobs:
  build:
    runs-on: ubuntu-latest
    if: github.ref != 'refs/heads/main'
    steps:
      -uses: shalzz/zola-deploy-action@v0.19.1
       with:
         build-dir: docs
         stage: build
         check-flags: --drafts
          
  build_and_deploy:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      -uses: shalzz/zola-deploy-action@v0.19.1
       with:
         stage: push
         check-flags: --drafts
         build-dir: docs
         pages-branch: gh-pages
```

## Input Variables
* `pages-branch`: default: `gh-pages` The git branch of your repo to which the built static files will be pushed.
* `target-repository`: default: `GITHUB_REPOSITORY`(current repository) The target repository to push/deploy to.
* `build-dir`: default: '.'. The path from the root of the repo where we should run the `zola build` command.
* `build-flags`: default: ''. Custom build flags that you want to pass to zola while building. (Be careful supplying a different build output directory might break the action).
* `stage`: [dryrun, check, build, push] default: push. How far to run the build 
* `submodules`: default: true. Checkout submodules (for themes)
* `skip-check`: default: false. Set to `true` to skipping checking external links with `zola check`. Build will still check internal
* `check-flags`: default: ''. Custom check flags that you want to pass to `zola check`.
* ~~`GITHUB_HOSTNAME`: The Github hostname to use in your action. This is to account for Enterprise instances where the base URL differs from the default, which is `github.com`.~~ We think this is now handled by doing everything through the official checkout action. Let us know if there's a problem with it.
* `clear-history`: default: false. DANGER- force push an orphan to clean history of target/gh-pages. Right now it's always clearing, but we're working on a fix

## Custom Domain

If you're using a custom domain for your GitHub Pages site put the CNAME 
in `static/CNAME` so that zola puts it in the root of the public folder
which is where GitHub expects it to be.

[zola]: https://github.com/getzola/zola

## Development
* ```git submodule add git@github.com:janbaudisch/zola-sam.git themes/zola-sam``` to test submodule inclusion of themes

##

Thanks and enjoy your day!

<a href="https://www.buymeacoffee.com/shaleen"><img src="https://img.buymeacoffee.com/button-api/?text=Buy me a Beer&emoji=🍺&slug=shaleen&button_colour=40DCA5&font_colour=ffffff&font_family=Bree&outline_colour=000000&coffee_colour=FFDD00" /></a>
