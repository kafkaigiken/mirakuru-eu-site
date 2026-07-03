source "https://rubygems.org"

# Plain Jekyll for local preview. We deliberately do NOT use the heavier
# `github-pages` gem: GitHub Pages builds the live site with its own gem
# environment and ignores this Gemfile, so it only affects local previews — and
# for a site this simple, plain Jekyll renders identically while pulling in far
# fewer (and lighter) native dependencies.
gem "jekyll", "~> 4.3"

# Ruby 3.0+ dropped webrick from the standard library and Jekyll's local server
# needs it. Jekyll 4.3 already depends on webrick; we pin it explicitly so a
# "cannot load such file -- webrick" error can never surprise anyone.
gem "webrick", "~> 1.8"
