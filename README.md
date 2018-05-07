# GitHub Pages test

This repository contains a [Minimal, Complete, and Verifiable example][mcve] about a suspected bug with GitHub Pages. This README file is a description. If you have anything to discuss, please navigate to the [Issues][issues] section of this repository.


# Background

GitHub officials published [a blog][1] announcing that it has since been possible to use an arbitrary open-source, GitHub-hosted Jekyll theme, by specifying a `remote_theme` key in `_config.yml` of a GitHub Pages repository, like this:

```yaml
remote_theme: pages-themes/cayman
```

However, I spent a lot of time debugging my GH Pages site because whenever I use a `remote_theme`, I get an email about page build failure like this:

> ### [iBug/gh-pages-test] Page build failure
>
> The page build failed for the `master` branch with the following error:
>
> The cayman theme is not currently supported on GitHub Pages. For more information, see <https://help.github.com/articles/adding-a-jekyll-theme-to-your-github-pages-site/>.
>
> For information on troubleshooting Jekyll see:
>
> <https://help.github.com/articles/troubleshooting-jekyll-builds>
>
> If you have any questions you can contact us by replying to this email.

After some intensive debugging, I found this possible problem cause.


# The Problem

According to [the `README.md` of an official theme][2], one can override the auto-generated download URLs by specifying the variables in their `_config.yml` like this:

```yaml
github:
  zip_url: https://github.com/iBug/gh-pages-test/archive/master.zip
  tar_url: https://github.com/iBug/gh-pages-test/archive/master.tar.gz
```

Unfortunately, this way of download URL overriding is **NOT** compatible with the `remote_theme` key. The presence of both keys in the configuration will cause an automatic "Page build failure" mail to be sent to you.

The [provided `_config.yml`][config] is an almost-minimal configuration file supplied for reproducing the problem.


# Reproducing the Problem

First, [fork this repository][fork]. Go to the Settings page of your forked repository, navigate to the "GitHub Pages" section and select "master branch" for the source. Then sit and wait for the "Page build failure" mail.

To build the site correctly, go to `_config.yml` and change the first line to:

```yaml
theme: jekyll-theme-cayman
```

You will see the site is built correctly, even though `jekyll-theme-cayman` is the same theme as [`pages-themes/cayman`][cayman].

### To verify this isn't an issue with the site itself

I have created a [`Gemfile`][gemfile] for building the site locally. All you need to do is installing Ruby, RubyGems and Bundler, then run a few simple commands in a terminal:

```
bundle install
bundle exec jekyll build
```

You can then verify that the site builds correctly into the `_site` folder in the working directory.



  [mcve]: https://stackoverflow.com/help/mcve
  [issues]: https://github.com/iBug/gh-pages-test/issues
  [fork]: https://github.com/iBug/gh-pages-test/fork
  [config]: https://github.com/iBug/gh-pages-test/blob/master/_config.yml
  [gemfile]: https://github.com/iBug/gh-pages-test/blob/master/Gemfile
  [cayman]: htps://github.com/pages-themes/cayman

  [1]: https://blog.github.com/2017-11-29-use-any-theme-with-github-pages/
  [2]: https://github.com/pages-themes/cayman#overriding-github-generated-urls
