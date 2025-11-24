# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

rubocop-fjord is a RuboCop configuration gem that provides standardized Ruby and Rails linting rules for Fjord, Inc. It packages two primary configuration files that are inherited by consuming projects.

## Architecture

This is a configuration-as-code gem with minimal Ruby code:

- **Configuration Files**: The core value is in `config/rubocop.yml` (base Ruby rules) and `config/rails.yml` (Rails-specific rules and exclusions)
- **Module Structure**: Simple module definition in `lib/rubocop/fjord.rb` with version in `lib/rubocop/fjord/version.rb`
- **Consumption Pattern**: Projects inherit these configurations via `inherit_gem:` in their `.rubocop.yml`

### Key Configuration Decisions

The base configuration (`config/rubocop.yml`):
- Requires `rubocop-performance` plugin
- Targets Ruby 3.2+, disables new cops by default
- Relaxes line length to 160 characters
- Disables documentation requirements and several metrics cops
- Excludes performance cops from tests

The Rails configuration (`config/rails.yml`):
- Requires `rubocop-rails` plugin
- Excludes common Rails directories (views, config, migrations, vendor, node_modules, etc.)
- Uses pattern `db/*schema.rb` to match both `schema.rb` and Rails 8's `schema_cache.rb`

## Development Commands

### Setup
```bash
bin/setup  # Install dependencies
```

### Testing
```bash
rake test           # Run all tests
bundle exec rake test  # Explicit bundler invocation
```

### Gem Management
```bash
bundle exec rake install   # Install gem locally for testing
bundle exec rake release   # Tag, build, and push to RubyGems (bumps version)
```

### Version Updates

1. Update version in `lib/rubocop/fjord/version.rb`
2. Run `bundle exec rake release` to tag and publish

## Testing Configuration Changes

To test configuration changes in a consuming project:
1. Make changes to `config/rubocop.yml` or `config/rails.yml`
2. Run `bundle exec rake install` to install locally
3. In consuming project, run `bundle update rubocop-fjord` (with local gem source)
4. Run `rubocop` to verify behavior
