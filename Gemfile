source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:

#     bundle exec jekyll serve

# Default theme for new Jekyll sites.
gem "minima", "~> 2.5"

# To upgrade in github, run `bundle update github-pages`.

bundle update github-pages
gem "github-pages", "~> 228", group: :jekyll_plugins

# Plugins:
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

gem "webrick", ">= 2.2.8"

gem "jekyll-sitemap", "~> 1.4"

gem "jekyll-seo-tag", "~> 2.8"

gem "jekyll-redirect-from", "~> 0.16.0"
