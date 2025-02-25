source "https://rubygems.org"

# Local deployment only
# gem "jekyll", "~> 4.3.2"

# To use github pages (upgrade with run `bundle update github-pages`)
gem "github-pages", "~> 228", group: :jekyll_plugins

# Default theme
gem "minima", "~> 2.5"

# Plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

gem "jekyll-sitemap", "~> 1.4"

gem "jekyll-seo-tag", "~> 2.8"

gem "jekyll-redirect-from", "~> 0.16.0"

gem "faraday", "~> 2.7"

gem "racc", "~> 1.6"

gem "nokogiri", "~> 1.18"

gem "i18n", "~> 1.12"
gem "minitest", "~> 5.18"
gem "activesupport", "~> 7.0"
gem "addressable", "~> 2.8"
