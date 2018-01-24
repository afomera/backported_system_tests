# Back-ported System Tests

In 2017, the company I work for wanted System Tests, but with 5.1 released we knew it would be a while before we could upgrade to Rails 5.1, because we were stuck on Rails 4.2. After working for a year, we are now running Rails 5, but wanted to ease the upgrade path for Rails 5.1 and still get the benefits of system tests.

But all the credit for this code should go to the Rails contributors, I didn't have to write any code to make this work. If any Rails contributors have an issue with this gem, please let me know on Twitter: [@AndrewFomera](https://twitter.com/AndrewFomera). I spoke with [eileencodes](https://github.com/eileencodes) at RubyConf 2017 in passing asking if it'd be okay to backport this, but if anyone has any issues let me know, happy to take it down.

## Table of contents

- [Credits](#credits)
- [Installation](#installation)
- [Configuration](#configuration)
- [Developing](#developing)
- [License](#license)


## Credits

Credit goes to all of the Rails Contributors that built the System Testing for Rails 5.1+.


## Installation

Add to your Gemfile:

```ruby
gem 'backported_system_tests'

# Ensure gem 'selenium-webdriver' is also in your Gemfile
# if using the selenium drivers.
```

Create `test/application_system_test_case.rb`

```ruby
require 'test_helper'

class ApplicationSystemTestCase < ActionDispatch::SystemTestCase
  driven_by :selenium, using: :chrome, screen_size: [1400, 1400]
  # Or you can change it to be using: :headless_chrome for headless chrome
end
```

Modify your application Rakefile for a `rails test:system` command.

**Rakefile**
```ruby
namespace :test do
  desc "Run only the tests in the `test/system` directory"
  task :system  => "test:prepare" do
    $: << "test"
    Rails::TestUnit::Runner.run(["test/system"])
  end
end
```

We also recommend creating another rake task for running your other tests, but will leave the implementation up to you.

**Note: This does not include the ActiveRecord changes, we recommend running your non-system tests in a separate process**

## Configuration

You should get all of the features of the adapters from Rails 5.1+, so you can switch out your adapters per the [Rails docs](http://api.rubyonrails.org/v5.1/classes/ActionDispatch/SystemTestCase.html).

Check the docs for more help, or open an issue :)

## Developing

1. Thank you! We love [our contributors](https://github.com/king601/backported_system_tests/graphs/contributors)!
1. Clone the repository.
1. Make your changes in a thoughtfully-named branch.
1. Ensure 1:1 test coverage.
1. Submit a [Pull Request](https://github.com/king601/backported_system_tests/pulls)!
1. Celebrate! :tada:

## License

This project uses MIT-LICENSE.
