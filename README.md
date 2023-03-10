# DDEV-Selenium Nightwatch Error

I think I've found an error with the standard setup
of [ddev-selenium-standalone-chrome](https://github.com/ddev/ddev-selenium-standalone-chrome). I'm not sure if this is
the right place to post this, but I'm hoping someone can help me out.

## Testing Instructions

1. Install a new ddev backed Drupal 10 site using the following instructions

```bash
mkdir my-drupal10-site
cd my-drupal10-site
ddev config --project-type=drupal10 --docroot=web --create-docroot
ddev get ddev/ddev-selenium-standalone-chrome
ddev start
ddev composer create drupal/recommended-project
ddev composer require drupal/core-dev --dev --update-with-all-dependencies
ddev composer require drush/drush
ddev drush site:install --account-name=admin --account-pass=admin -y
```

2. Setup Nightwatch and run a test that should succeed

```bash
ddev exec -d /var/www/html/web/core yarn install
ddev exec -d /var/www/html/web/core touch .env
ddev exec -d /var/www/html/web/core yarn test:nightwatch tests/Drupal/Nightwatch/Tests/exampleTest.js
```

3. Run a Nightwatch test that installs a module. This should fail.

```bash
 ddev exec -d /var/www/html/web/core yarn test:nightwatch ./modules/toolbar/tests/src/Nightwatch/Tests/toolbarApiTest.js
```

## Probably causes

When I watch this test run, using VNC, I see that browser closes after it tries to install the breakpoint module. Having
the browser be closed when the remaining tests run seem to lead to a lot of failures.

## Unanswered questions

Is this happening for others?

If this is happening to a core test why isn't core failing to pass tests?

Is the fact that this addon uses seleniarm/standalone-chromium:4.1.4-20220429 and not drupalci/chromedriver:production a
contributing factor?
