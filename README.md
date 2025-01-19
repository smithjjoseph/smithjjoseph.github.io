# Portfolio website

## Installation

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
   
> NOTE: `--incremental` is used to enable incremental rebuilds (which speeds up subsequent builds) however this seems to not update the index page and therefore should only be used when editing blogs