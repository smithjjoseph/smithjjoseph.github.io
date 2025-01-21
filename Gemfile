source "https://rubygems.org"

# Manage to ensure which Jekyll version is used to run.
# After changing working version below, save and rerun bundle
gem "jekyll", "~> 4.3.4"

# Extra plugins
group :jekyll_plugins do
  gem "jekyll-seo-tag", "~> 2.6"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
