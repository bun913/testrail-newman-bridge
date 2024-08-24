# newman-reporter-testrail-neo

> [!NOTE]
> This repository is inspired by [billylam/newman-reporter-testrail](https://github.com/billylam/newman-reporter-testrail).
> Unfortunately, the project has been archived and is no longer maintained.
> So, I decided to create a new project based on the original one.(not forked)

TestRail reporter for Newman.

## Installation

WIP - Not yet published to npm.

## Usage

### Prefix all test assertions you wish to map with the test number.
Include the letter C. You may map more than one test case to an assertion.
```
pm.test("C226750 C226746 Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

### Export the following environment variables.

#### REQUIRED Environment Variables

| Name | Description |
| --- | --- |
| TESTRAIL_DOMAIN | TestRail domain.  Do not include protocol. |
| TESTRAIL_USERNAME | TestRail username / email. |
| TESTRAIL_APIKEY | TestRail [API key](http://docs.gurock.com/testrail-api2/accessing#username_and_api_key). |
| TESTRAIL_PROJECTID | TestRail project id. |

#### OPTIONAL Environment Variables
| Name                    | Description                                                                                                                                                                                                         |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TESTRAIL_RUNID          | TestRail run id.  Update a specific run instead of creating a new run.  Can use the string "latest" to update latest run.                                                                                           |
| TESTRAIL_SUITEID        | TestRail suite id.  Mandatory in multi-suite projects.  Do not use in single-suite projects.                                                                                                                        |
| TESTRAIL_MILESTONEID    | Milestone to link test to.
| TESTRAIL_PLANID           | Test Plan Id to add run to. NOTE: Requires TESTRAIL_SUITEID to be set even in single-suite projects.                                                                                                                                                                                          |
| TESTRAIL_VERSION        | Version of API tested.                                                                                                                                                                                              |
| TESTRAIL_TITLE          | Title of test run to create.             
| TESTRAIL_REFS          | Ref of test run to create.                                                                                                                                                                                        |
| TESTRAIL_INCLUDEALL     | Whether to include all tests in run, regardless of whether actually run by Newman.  Defaults to true.                                                                                                               |
| TESTRAIL_CLOSE_RUN      | Whether to close the TestRail run after completion of the Newman run.  Defaults to false.                                                                                                               |
| TESTRAIL_CUSTOM_*       | A fixed testrail field, where * is the field key                                                                                                                                                                    |
| TESTRAIL_LOGGING        | Output logging.  Options are full, headers, none.  Defaults to full.  Use the none or headers option if you are getting a "Request Entity Too Large" error.                                                         |
| TESTRAIL_PASSED_ID      | The ID of a custom status to use for Passed.  Defaults to 1 which is the value for the Passed status. |                                                                                                           
| TESTRAIL_FAILED_ID      | The ID of a custom status to use for Failed.  Defaults to 5 which is the value for the Failed status.                                                                                                               |
| TESTRAIL_SKIPPED_ID     | The ID of a custom status to use for Skipped.  Defaults to 4 which is the value for the Skipped status.                                                                                                             |
| TESTRAIL_STEPS          | Project uses test steps.  Test cases that share the same case id are assumed to be steps for the same test case.  Defaults to false.                                                                                |
| TESTRAIL_STEPRESULT_KEY | If TESTRAIL_STEPS is set to true and your TestRail configuration changed the Step Results field to another value then you can set this env variable to the correct name.  By default this is set to "step_results". |
| TESTRAIL_TITLE_MATCHING | Attempt to fetch test cases from server and match via case name instead of id                                                                                                                                       |
| TESTRAIL_BETA_API       | Set to true to add additional *X-API-IDENT: BETA* header. Can be needed in order to use reporter with older TestRail versions (before Feb. 21, 2021). Defaults to false.                                            |

You can use [direnv](https://github.com/direnv/direnv) to easily maintain directory-specific options.

You may also set some or all of these variables using bash exports or by declaring directly in the run command.

### Run newman with the reporter option

`-r testrail`

Example:

```
TESTRAIL_DOMAIN=example.testrail.com TESTRAIL_USERNAME=exampleuser 
TESTRAIL_APIKEY=yourkey TESTRAIL_PROJECTID=99 TESTRAIL_TITLE="Dev-API Regression" 
newman run my-collection.postman_collection.json -r testrail,cli
```

## Development

> [!NOTE]
> I'd like to automate the system test process, but TestRail doesn't have any free plans.
> So, it's difficult to prepare the test environment.
> Currently, I'm using my own TestRail account to test the system test.

### How to System Test

1. export environment variables

```bash
export TESTRAIL_DOMAIN=hoge.com
export TESTRAIL_USERNAME=hoge@example.com
export TESTRAIL_PROJECTID=1
export TESTRAIL_APIKEY=fugafuga
```

2. pack this package

```bash
npm pack
```

3. install the package

```bash
# x.x.x is the version of the package
npm i -g ./newman-reporter-testrail-neo-x.x.x.tg
```

4. run system test

```bash
newman run ./test/systemTest/system_test.json -r testrail-neo
```
