# DataTrue API client

DataTrue is a SaaS platform to audit, monitor and validate tags, dataLayers and data collected from websites. The [DataTrue Test Builder chrome extension](https://chrome.google.com/webstore/detail/datatrue-test-builder/oghoceohpfhokhcoomihkobmpbcljall?hl=en) can quickly create test interactions with websites using our library of 100+ tag templates or custom tags. DataTrue works across complex AJAX interactions (e.g. using [AngularJS](https://angularjs.org/) and iframe content.

This ruby client allows you to trigger DataTrue tests from a Continuous Integration tool such as Jenkins, Teamcity, Travis-CI, Codeship and others.  If you’re practicing Continuous Delivery, it can be used to trigger a test of your application as soon as changes are released.

## Usage

You will need a DataTrue account ([free sign-up](https://datatrue.com/)) to use this gem.  To get your API key go to the [Accounts page](https://datatrue.com/accounts/), select your account and click on "Generate API Key".

The next steps assume you have a test suite created in DataTrue.  Read our [Knowledge Base](https://support.datatrue.com/hc/en-us/categories/200080049-Knowledge-Base) to find-out [how to quickly create a single-page test](https://support.datatrue.com/hc/en-us/articles/213538568-1-Use-Quick-Start-to-create-a-single-page-test).

Install the gem on the system you want to trigger your tests from:

    $ gem install datatrue_client

Alternatively, if you want to include the client as part of your ruby application, you can add this line to your Gemfile:

```ruby
gem 'datatrue_client', :group => [:test, :development]
```

### Command-line usage

Use the [DataTrue API wizard](https://datatrue.com/) to select your test(s) or test suite along with other options.  Paste the command-line in your terminal to see it in action; e.g.:

```
datatrue_client run 1539 -a rtTlaqucG9RrTg1G2L1O0u -t suite \
    -v HOSTNAME=datatrue.com,GTMID=GTM-ABCXYZ \
    -e 543,544

datatrue_client: job=5e9316aa116b4a6fe5dfebda68accd60 created for test="DataTrue Public pages"
datatrue_client: job=5e9316aa116b4a6fe5dfebda68accd60 started as test_run_id=52454
datatrue_client: test_run_id=52454 step=1 total_steps=7 result=running
datatrue_client: test_run_id=52454 step=1 total_steps=7 result=passed
...
datatrue_client: test_run_id=52454 step=7 total_steps=7 result=passed
datatrue_client: test_run_id=52454 finished result=passed.
```

The exit status of the application will change according to test results:
* `0`: test run successful, result=passed.
* `1`: test run successful, result=failed.
* `-1`: generic test run error. See output detail.
* `-2`: authentication or authorisation error.  Check your API key and test identifiers.
* `-3`: quota exceeded.  You have used-up all your subscription allowance for this period.

If you want to ignore the exit status, use the shell's `||` operator; e.g.: `datatrue_client [options] || true`.  This will ensure that the exit status is always `0`.

`datatrue_client <command> [command-arguments] -a <api_key> [command-options]`

_Commands_:

* `run`: triggers a new run of tests or a test suite and waits for it to finish.

```text
datatrue_client run <suite_id | test_id_1,test_id_2,...> -a <api_key>
    [-t | --type=suite|test] [-v | --variables foo=bar,thunder=flash]
    [-e | --email-users '1,2,3...'] [-o | --output [filename]] [-s | --silent]
```    

* `trigger`: triggers a new run of tests or a test suite and exits immediately.

```text
datatrue_client trigger <suite_id | test_id_1,test_id_2,...> -a <api_key>
    [-t | --type=suite|test] [-v | --variables foo=bar,thunder=flash]
     [-s | --silent]
```

* `poll`: queries DataTrue for results of a test run at a regular interval until it's finished.

`datatrue_client poll <job_id> -a <api_key>`

* `results`: retrieves the results of a test run from DataTrue.

`datatrue_client results <test_run_id> -a <api_key> [-o | --output [filename]] [-s | --silent]`

_Options_:
* `-a`: mandatory. The DataTrue API key.  This overrides the API key provided as an environment variable.
* `-t` or `--type`: the type of test to be run.  Valid options are `suite` or `test`.
* `-v` or `--variables`: run-time variables to be provided to the test.  These can be used to change behaviour of your test, provide credentials and more.
* `-e` or `--email-users`: a comma-separated list of user identifiers who will receive an email with the test results.
* `-o` or `--output`: write the test results as a JUnit XML report that can be used to integrate DataTrue test results with other test tools (e.g. Jenkins).  If no filename is provided the client will create a `<job_id>.xml`.
* `-s` or `--silent`: suppress all application output.

#### Environment variables

* `DATATRUE_API_KEY`: your DataTrue API key.  The `-a` option takes precedence.

### Usage in a Ruby application

### Jenkins Integration

The DataTrue client can output test results in the [JUnit  format](https://github.com/windyroad/JUnit-Schema/blob/master/JUnit.xsd) which can then be parsed by the [Jenkins JUnit plugin](https://wiki.jenkins-ci.org/display/JENKINS/JUnit+Plugin) and incorporated into your test results.

Here's an example of what the results look like in Jenkins v1.6.

<img src="documentation/jenkins_datatrue_test_result_summary.png?raw=true" alt="DataTrue test result summary in Jenkins" height="400"/>


## Support

Our [support website](https://support.datatrue.com/) has more detailed information about DataTrue and the API client.

If you believe you have found a bug, please [reach-out using the support website](https://support.datatrue.com/hc/en-us/requests/new) or through support@datatrue.com.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/Lens10/datatrue_client.


### Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
