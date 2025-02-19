![Jenkins](https://img.shields.io/jenkins/build?jobUrl=https%3A%2F%2Fci-builds.apache.org%2Fjob%2FCamel%2Fjob%2FCamel.website%2Fjob%2Fmain%2F&label=Master%20build)

# Apache Camel Website <img alt="Apache Camel" src="antora-ui-camel/src/img/logo-d.svg" height="30 px">

This is a site generator project for Apache Camel. It generates static HTML and
resources that are to be published.

Tools used to generate the website:
 - [Git](https://git-scm.com/) a source code management tool used to fetch document sources from different
   github repositories.
 - [Node.js](https://nodejs.org/) a JavaScript runtime used to build the website. You will need to use Node.js version 10.
 - [yarn](https://yarnpkg.com/) a blazing fast dependency and package manager tool used to download
   and manage required libraries.
 - (installed via yarn) [Gulp](http://gulpjs.com/) a task automation tool. Used to build the Camel
   Antora UI theme.
 - (installed via yarn) [Hugo](https://gohugo.io) a static site generator. Simplified, it takes the
   documentation from the `content` directory and applies templates from `layouts`
   directory and together with any resources in `static` directory generates output in
   the `public` directory.
 - (installed via yarn) [Antora](https://antora.org/) a documentation site generator. It uses
   Asciidoc documents from different sources in the [Camel](https://github.com/apache/camel),
   [Camel K](https://github.com/apache/camel-k) and [Camel Quarkus](https://github.com/apache/camel-quarkus)
   repositories where user manual and component reference documentation resides and renders them for inclusion in this
   website.
 - (optional) [Maven](https://maven.apache.org/) a build tool used to run the complete website generating process.

## Build with Node and yarn

You can build the website locally using the tools `Node.js` and `yarn`.

If you can not use these tools on your local machine for some reason you can also build the website using Maven as
described in section ["Build with Maven"](#build-with-maven).

### Preparing the tools

### Chocolatey

For windows users, a beginning step to install yarn and nvm on your local system is through installing chocolatey.

An easy step to step guide to install chocolatey on your local system is as follows:
1. Open cmd/powershell and run it as administrator.

2. Install with cmd.exe

        > @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command " [System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

3. Install with PowerShell.exe

        > Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

#### Node

Make sure that you have Node.js (herein "`Node`") installed.

    $ node --version

If this command fails with an error, you do not have Node installed.

This project requires the Node LTS version 14 (e.g., v14.15.0).

Please make sure to have a suitable version of Node installed. You have several options to install
Node on your machine.

### Installation of nvm on Linux/Mac OS

- Install using the Node version manager [nvm](https://github.com/creationix/nvm)
- Install using [Homebrew](https://brew.sh/) and [Node formulae](https://formulae.brew.sh/formula/node)
- Install from official [Node packages](https://nodejs.org/en/download/)

An easy step to step guide to install nvm and install node v14 on your local system is as follows:

    $ touch ~/.bash_profile
    $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.36.0/install.sh | bash
    $ source ~/.nvm/nvm.sh
    $ nvm install 14


Note - If you have different Node version other than Node LTS version 14 you can use following command to make
Node LTS version 14 as default Node version.

    $ nvm use 14

### Installation of nvm on Windows

Note - The following steps need to be ran on cmd as administrator only.

An easy step to step guide to install nvm and install node v14 on your local system is as follows:

    > choco install nvm
    > nvm install 14

Note - If you have different Node version other than Node LTS version 14 you can use following command to make
Node LTS version 14 as default Node version.

    > nvm use 14


Now that you have Node 14 installed, you can proceed with checking the Yarn installation.

#### Yarn

Follow [the documentation on installing](https://yarnpkg.com/en/docs/install) Yarn for your Operating system.

> Note: For windows users, run on cmd as administrator and install yarn through chocolatey.

#### Clone and Initialize the project

Clone the Apache Camel Website project using git:

    $ git clone https://github.com/apache/camel-website.git

The command above clones the Apache Camel Website project. After that you can switch to the new
project directory on your filesystem.

## Build the website and Antora theme

Some of the content for the website is derived from the data received from GitHub API and rate limits can cause
build failures. For that reason it is necessary to set the following environment variables:

 - `HUGO_PARAMS_GitHubUsername=<GitHub username>`
 - `HUGO_PARAMS_GitHubToken=<GitHub token>`

These values are used by Hugo when building or running in development mode (`yarn preview:hugo`) or building the
website (`yarn build:hugo` or `yarn build-all`) to access GitHub API with a higher rate limit.

We're using yarn [workspaces](https://yarnpkg.com/features/workspaces) to build both the theme and the website run `build-all` script, for example:

    $ yarn build-all

That will build the Antora theme (from antora-ui-camel directory) and the website. Result of the build can be seen in the `public` directory.

## Build the Antora Camel UI theme

The theme sources are located inside [Project root directory/antora-ui-camel](antora-ui-camel). So first switch to that directory:

    $ cd antora-ui-camel

In that directory execute:

    $ yarn build   # to perform the ui theme build

You should see the Antora theme bundle generated in in `antora-ui-camel/build/ui-bundle.zip`.

In case `yarn build` raises error, run `yarn format` to format the code and re-run `yarn build` to build your bundle successfully.

The Camel Antora UI theme should not be a subject to change very frequently. So you might execute this once and
never come back.

## Build the website content

Building the website requires the built Antora Camel UI theme bundle from above. Please check that
the theme bundle exists in [antora-ui-camel/build/ui-bundle.zip](antora-ui-camel).

To build the website go to the project root directory and run:

    $ yarn build   # to perform the build


In case `yarn build` throws the error: **JavaScript heap out of memory**, the issue can be resolved by increasing the memory used by node.js by setting `NODE_OPTIONS` environment variable to include `--max_old_space_size`, for example to increase the old space to 4GB do:

```shell
$ export NODE_OPTIONS="--max_old_space_size=4096"
```
This should fetch doc sources for [Camel](https://github.com/apache/camel) and [Camel K](https://github.com/apache/camel-k)
and generate the website with Hugo. You should see the generated website in the `public` directory.

## Preview website locally

You can preview the Apache Camel website on your local machine once you have the generated website available in
the `public` directory.

Hugo can start a simple web server serving the generated site content so you can view it in your favorite browser.

Simply call

    $ yarn preview

and you will be provided with a web server running the site on [http://localhost:1313/](http://localhost:1313/)

Point your favorite browser to `http://localhost:1313/` and you will see the Apache Camel website.

Changes that are made to the content managed by Hugo (i.e. content, layouts, config.toml) are applied automatically and reloaded in the browser. To make changes to the content managed by Antora, a rebuild needs to be done. The same is true for the CSS changes in the `antora-ui-camel`. To rebuild you can run, in another terminal window, from the root directory of the website:

    $ (cd antora-ui-camel && yarn build) && yarn antora  --clean --fetch antora-playbook.yml

This will build the `antora-ui-camel` which holds all the CSS and JavaScript, and then rebuild the documentation, resulting in an updated content in the `documentation` directory.

To iterate quickly, it's easier to make changes directly in the browser tooling and then bring the changes over to the CSS files after the fact.

## Working on documentation (asciidoc) content

To build the documentation we pull the content of git repositories (see `antora-playbook.yml`), so to make changes locally and have them built or previewed without those changes being merged to Camel git repositories you need to adapt the `antora-playbook.yml` file.

For example to work on the user manual locally change the `content`, `sources` to point to `HEAD` of your local git repository, in this example located in `../camel`:

```yaml
content:
  sources:

    - url: ../camel
      branches: HEAD
      start_paths:
        # manual
        - docs/user-manual
```

Now you can run `yarn build:antora` or `yarn preview` and see the locally made changes. More details on this you can find in the Antora [documentation](https://docs.antora.org/antora/2.3/playbook/content-source-url/#local-urls).

Typical workflow is to run the `yarn preview` in one command line session and then rebuild the Antora documentation by running `yarn build:antora` for the documentation to be refreshed.

TIP: We pull in several git repositories and build several versions (branches) of documentation from them, time can be saved by removing sources for the documentation not worked on. Though be careful about inter-dependencies, for example several documents in the component reference point to the user manual.

## CAMEL_ENV environment variable

Setting the `CAMEL_ENV` changes the output of the website build slightly, possible values are `development` (set by default if `CAMEL_ENV` is unset), `production` or `netlify`.

To run the optimizations, which currently consist of running [htmlmin](https://kangax.github.io/html-minifier/) to reduce the size of generated HTML documents, set the `CAMEL_ENV` environment variable to `production`, for example:

    $ CAMEL_ENV=production yarn build

When build is performed on Netlify, we set it to `netlify` to add the link to Netlify required by Netlify's open source policy.

## Contribute changes

The Apache Camel website is composed of different sources. So where to add and contribute changes in particular?

### Changes on the website

#### Menu

The site main menu is defined in the top level configuration [config.toml](config.toml). You can add/change
menu items there.

#### Content

The basic website content is located in [content](content). You can find several different directorys representing different
areas of the website:

- [docs](content/docs): Getting started, user manual, component reference
- [download](content/download): Download Camel artifacts
- [blog](content/blog): Blog posts
- [community](content/community): Support, contributing, articles, etc.
- [projects](content/projects): Subproject information (e.g. Camel K)
- [security](content/security): Security information and advisories
- [releases](content/releases): Release notes

#### Adding new blog post

Use the `blog` archetype to create a new markdown content file in `content/blog`:

    $ yarn hugo new --kind blog blog/YYYY/MM/PostName/index.md # replace YYYY with the year, MM with the month and PostName with the actual name

Put a nice featured image in `content/blog/YYYY/MM/PostName/featured.png` and edit `content/blog/YYYY/MM/PostName/index.md` filling in the details.

The final generated URL would be something like `https://camel.apache.org/blog/2020/05/MyNewPost/`.

Don't forget to remove `draft: true` to publish the blog post.

#### Adding new security advisory content

Use the `security-advisory` archetype to create a new markdown content file in `content/security`:

    $ yarn run hugo new --kind security-advisory security/CVE-YYYY-NNNNN # replace YYYY-NNNNN with the CVE number

This will create a `content/security/CVE-YYYY-NNNNN.md` file which you need to edit to and fill in the required parameters.
The content of the created markdown file is added to the _Notes_ section.

Place the signed PGP advisory in plain text as `content/security/CVE-YYYY-NNNNN.txt.asc`.

Make sure that you set the `draft: false` property to have the page published.

#### Adding new release note

##### Release of Apache Camel (core)

Use the `release-note` archetype to create a new markdown content file in `content/releases`:

    $ yarn run hugo new --kind release-note releases/release-x.y.z.md # replace x.y.z with the release version

This will create a `content/release-x.y.z.md` file which you need to edit to and fill in the required parameters.
The content of the created markdown file is added to the _New and Noteworthy_ section.

Make sure that you set the `draft: false` property to have the page published.

##### Sub-project release

Use the `release-{category}` archetype to create a new markdown content file in `content/releases/{category}-{version with underscores}/index.md`.

For `{category} you can use: `k` `k-runtime`, `ckc`, or `q`.

For example, to create Camel Kafka Connector release note:

    $ yarn run hugo new --kind release-ckc releases/ckc/release-x.y.z.md # replace x.y.z with the release version (use underscores)

This will create a `content/releases/ckc/release-x.y.z.md` file which you need to edit to and fill in the required parameters.
The content of the created markdown file is added to the _New and Noteworthy_ section.

Make sure that you set the `draft: false` property to have the page published.

#### Layout and templates

Layout related changes go into [layout](layout) directory where you will find HTML templates that define the common layout
of the different page categories including footer and header templates.

### Changes in Antora UI theme

The Antora UI theme basically defines the look and feel of the website. You can find the theme sources within this
repository in [antora-ui-camel](antora-ui-camel).

You need to rebuild the Antora UI theme in order to see your changes reflected locally.

### Changes for Camel and Camel K docs

The Apache Camel website includes documentation sources from other github repositories. Content sources are defined in
[antora-playbook.yml](antora-playbook.yml).

At the moment these are the documentation sources from [Camel](https://github.com/apache/camel)
and [Camel K](https://github.com/apache/camel-k). These are basically the component reference docs and the Camel user
manual. In case you want to change something here, please go to the respective github repository and contribute your
change there.

- [Camel components](https://github.com/apache/camel/tree/master/docs/components)
- [Camel user manual](https://github.com/apache/camel/tree/master/docs/user-manual)
- [Camel K docs](https://github.com/apache/camel-k/tree/antora/docs)

Your changes in these repositories will automatically get visible on the website after a site rebuild.

## Build with Maven

The project provides a simple way to build the website sources locally using the build tool [Maven](https://maven.apache.org/).

The Maven build automatically downloads the tool binaries such as `node` and `yarn` for you. You do not need to install
those tools on your host then. The binaries are added to the local project sources only and generate the website content.

As the Maven build uses pinned versions of `node` and `yarn` that are tested to build the website you most likely avoid
build errors due to incompatible versions of `Node.js` tooling installed on your machine.

### Preparing Maven

Make sure that you have Maven installed.

    $ mvn --version

If this command fails with an error, you do not have Maven installed.

Please install Maven using your favorite package manager (like [Homebrew](https://brew.sh/)) or from
official [Maven binaries](https://maven.apache.org/install.html)

### Building from scratch

When building everything from scratch the build executes following steps:

- Download `yarn` and `Node.js` binaries to the local project
- Load required libraries to the local project using `yarn`
- Build the Antora Camel UI theme ([antora-ui-camel](antora-ui-camel))
- Fetch the doc sources from [Camel](https://github.com/apache/camel)
  and [Camel K](https://github.com/apache/camel-k) github reporsitories
- Build the website content using Hugo

You can do all of this with one single command:

    $ mvn package

The whole process takes up to five minutes (time to grab some coffee!)

When the build is finished you should see the generated website in the `public` directory.

### Rebuild website

When rebuilding the website you can optimize the build process as some of the steps are only required for a fresh
build from scratch. You can skip the ui theme rendering (unless you have changes in the theme itself).

    $ mvn package -Dskip.theme

This should save you some minutes in the build process. You can find the updated website content in the `public` directory.

### Clean build

When rebuilding the website the process uses some cached content (e.g. the fetched doc sources for
[Camel](https://github.com/apache/camel) and [Camel K](https://github.com/apache/camel-k) or the Antora ui theme).
If you want to start from scratch for some reason you can simply add the `clean` operation to the build which removes
all generated sources in the project first.

    $ mvn clean package

Of course this then takes some more time than an optimized rebuild (time to grab another coffee!).

# Checks, publishing the website

The content of the website, as built by the [Camel.website](https://ci-builds.apache.org/job/Camel/job/Camel.website/job/main/)
job, is served from the [asf-site](https://github.com/apache/camel-website/tree/asf-site) branch and served by [ASF
Infrastrucuture team](https://infra.apache.org/project-site.html).

For the site to be published a number of checks need to pass, these include two levels of link checking:
[Antora xref](https://gitlab.com/antora/xref-validator) and [HTML link checker](https://github.com/deadlinks/cargo-deadlinks);
[HTML validation](https://html-validate.org/). In local development those checks can be run with `yarn checks`,
to increase the local turnaround time some lengthy checks are not run unless `CAMEL_ENV=production` environment
variable is set. To run all checks use:

    $ CAMEL_ENV=production yarn checks

Publishing the website is done by [ASF Jenkins](https://ci-builds.apache.org/job/Camel/job/Camel.website/job/main/)
this includes running all the checks. It is common that a check might fail there that hasn't failed when the
change was made, this could be for any number of reasons, but most commonly there was a change in one of
the subproject's documentation, and most common issue is an introduction of a broken or absolute link towards
`camel.apache.org` domnain.

The configuration of the HTML validation rules is in `.htmlvalidate.json`, with exclusions of checks listed in
`.htmlvalidateignore` file, custom rules are in `rules.js` for mandating relative links to camel.apache.org domain,
JSON-LD schema validation, and mandating that the HTML title be set.

## Pull request previews are powered by Netlify

This website is hosted by Apache Software foundation. Pull request previews and checks are powered by Netlify.

![](https://www.netlify.com/img/global/badges/netlify-light.svg "Deploys by Netlify")
