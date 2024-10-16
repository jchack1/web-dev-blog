source "https://rubygems.org"

gem "jekyll", "~> 3.7.4"
gem "github-pages", "~> 192"
gem "rake", "~> 12.3.1"

# this is to solve a deployment error I got in Oct 2024
# error was: ... requires rubygems version >= 3.3.22, which is incompatible with the current version, 3.0.6
# fix came from here: https://github.com/orgs/community/discussions/127006
# not sure why this works but I think it's because netlify updated a dependency so we had to explicitly use a lower version in this project
# might need to update dependencies at some point
gem "ffi", "~> 1.16.3"