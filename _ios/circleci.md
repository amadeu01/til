# TIL: CircleCI and iOS UI Testing

I spent my last holiday trying to understand and navigate through CircleCI and iOS UI testing. My attempt to improve the CI time and split the tests did not meet my expectations 😅. Here are my discoveries:

## 1. CircleCI CLI is Poorly Documented
The CircleCI CLI has almost no support for iOS, and their documentation hasn't been updated in years. Finding reliable information was a challenge.

## 2. Splitting Tests Using CircleCI CLI

If you want to split your tests efficiently, you can use the following CircleCI CLI command:

```bash
circleci tests glob MyAppUITests/*.swift | sed 's@/@.@g' | sed 's/.swift//' \
  | circleci tests run --split-by=timings --timings-type=classname --command="> dist/test_classes_to_run.txt xargs echo" --verbose
```

### Explanation:
- The command `> dist/test_classes_to_run.txt xargs echo` writes the output of the split tests into `dist/test_classes_to_run.txt`.
- `circleci tests glob` helps create a pattern of files/classes to be used for running tests.
- I initially tried avoiding `circleci tests glob`, but without it, the split did not work as expected.
- The key part is using `--timings-type=classname` so that tests are split based on previous execution times, allowing them to be distributed more evenly across machines.

## 3. Using `circleci tests run` vs `circleci split tests`
The `circleci tests run` command is recommended over `circleci split tests` because it provides additional features like rerunning failed tests.

> *Use the `circleci tests run` command to run your tests, split your tests across parallel executors, and take advantage of the rerun failed tests options.*

> *Two commands are available for you to split your tests. We recommend using `circleci tests run` as this enables you to also take advantage of the rerun failed tests feature. The examples in this section use the `circleci tests run` command.*

[Source: CircleCI Documentation](https://circleci.com/docs/use-the-circleci-cli-to-split-tests/#use-circleci-tests-run)

## 4. Use Fastlane for Easier Test Execution
Using [Fastlane](https://fastlane.tools/) can simplify building and compiling the code shared across virtual machines running the UI tests.

## 5. Storing and Retrieving Test Classnames
The best approach I've found to store and retrieve test class names (UI test slugs in CircleCI) is:
- Maintain Swift files with the same name as the test classes.
- Try to keep just one test class per file.
- Navigate to the **Test Insights** tab in CircleCI to check the `classname` used for each test saved in CircleCI. This allows you to double-check if your script is generating the correct class names for retrieving test times accurately.

---
That was my journey with CircleCI and iOS UI testing. Hopefully, this helps someone avoid the pitfalls I encountered! 🚀
