source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
# Project tested with Jekyll 4.4.x. Bump made to allow local system
# installations of Jekyll 4.4 while still staying compatible with GitHub
# Pages where appropriate. After changing this, run `bundle install`.
gem "jekyll", "~>4.4"
gem "rake", "~>13.0"

# Required to load Windows-only dependencies
# and avoid unnecessary noise on non-Windows environments
if Gem.win_platform?
  platforms :windows do
    gem "csv"
    gem "base64"
  end
end

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins

# If you have any plugins, put them here!
group :development do
  gem "jekyll-compose", "~> 0.12"
  gem "html-proofer", "~> 5.1"
  gem "webrick", "~> 1.8"
end

group :jekyll_plugins do
  gem "jekyll-feed", "~>0.17"
  gem "jekyll-paginate-v2", "~>3.0"
  gem "jekyll-seo-tag", "~>2.8"
end
