# Ruby Stack Profile

## Import Patterns
- `require` vs `require_relative` — check consistency
- Autoloading (Zeitwerk vs classic) — check `config/application.rb`
- Check for circular dependency issues in initialization

## Package Manager
- `Gemfile` / `Gemfile.lock` → Bundler
- Check Ruby version in `.ruby-version` or Gemfile
- Check for vendored gems (`vendor/bundle`)

## Framework Signals
- `config/routes.rb` → Rails (check version)
- Sinatra / Hanami / Roda → lighter frameworks
- `Rakefile` → task definitions
- Sidekiq / Resque / Delayed Job → background job processing

## Legacy Indicators
- Ruby version < 3.0 → older patterns
- Rails version in Gemfile (4.x, 5.x, 6.x, 7.x) → era indicator
- `protected_attributes` gem → pre-strong-params
- Asset pipeline (`sprockets`) alongside Webpacker/Propshaft → asset migration
- `HashWithIndifferentAccess` usage → potential Symbol/String inconsistency
- Callbacks-heavy models → design pattern concern

## Configuration Surfaces
- `config/` directory (Rails)
- `config/database.yml`
- `config/environments/*.rb`
- `.env` files (dotenv gem)
- `config/initializers/` — check for dead initializers

## Testing Conventions
- RSpec vs Minitest — check which is primary
- FactoryBot / fixtures — test data strategy
- Check for feature/system specs (Capybara)
- `spec_helper.rb` vs `rails_helper.rb` organization
- VCR / WebMock for HTTP stubbing

## Common Migration Patterns
- Rails version upgrades: Check deprecation warnings, removed APIs
- Sprockets → Webpacker → Import Maps/Propshaft
- ERB → ViewComponent / Phlex → component-based views
- Minitest → RSpec or vice versa
- Classic autoloader → Zeitwerk
