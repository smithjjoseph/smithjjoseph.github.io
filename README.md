# Portfolio website

## Github Pages

Initial version of site just used plain HTML and CSS files

### Build from branch

- Moved to using Jekyll for modularity, pluggins, and sass
- Used 'build from branch' which used the Github Pages gem for continuous integration

### Github Actions

Main issue:
- Development environment using Jekyll 4.3.4
- Github Pages gem uses Jekyll 3.10.0

Reasoning:
- Remedied developement environment deprecation warnings relating to using `@use` and `@forward` instead of `@import`
- Worked in development environment but broke the deployed website's styling
- Updated the gemfile to use a specific version of Jekyll (same version as development environment)
- Used Github Actions instead of 'build from branch' for continuous integration
- Uses the default Jekyll workflow

### DNS Configuration for custom URL

Commands for checking DNS configuration as it can take up to 48 hours for changes to propagate - Use WSL for dig command

`dig _github-pages-challenge-smithjjoseph.josephjamessmith.com +nostats +nocomments +nocmd TXT` \
(Should display the inputted record)

`dig example.com +nostats +nocomments +nocmd` \
(Should point to GitHub's servers)

`dig www.example.com +nostats +nocomments +nocmd` \
(Should point to GitHub which should in turn point to GitHub's servers)

## Development Environment Installation

### WSL

Using WSL for containerisation

```bat
:: Uninstall old copy of Ubuntu
wsl -l -v
wsl --unregister Ubuntu

:: Install new copy of Ubuntu
wsl --install -d Ubuntu
wsl
sudo apt update
sudo apt full-upgrade -y
```

### Jekyll

Requirements: Ruby >2.5.0, RubyGems, GCC, Make
Source: https://jekyllrb.com/docs/installation/ubuntu/

```bash
# Install prerequisites
sudo apt update
sudo apt install build-essential ruby-full zlib1g-dev

# Create a non root path for ruby gems
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Install jekyll
gem install jekyll bundler
```

## Project Creation

1. Change directory to project directory
1. Create a new jekyll project using `jekyll new [sitename]`
1. Activate the site using `bundle exec jekyll serve --livereload --force-polling`
   - `--livereload` to make live changes to site without needing to refresh
   - `--force-polling` may not be needed but for my installation seemed to be necessary

> NOTE: `--incremental` is used to enable incremental rebuilds (which speeds up subsequent builds) however this seems to not update the index page and therefore should only be used when editing posts