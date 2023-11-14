# Assignment Submission

## Project Selected: PYTEST

## I. Introduction
- This section's purpose is to detail tests runTest_method. andtest_early_skip in Pytest

## II. Test Case 1: [def test_runTest_method(pytester: Pytester) -> None:] in test_unittest.py
### A. Description
- The purpose of this test is to verify if the Pytester can create a test file, run it, and get the correct output out of the test file.
### B. Gherkin Syntax (if applicable)
- If you choose to use Gherkin syntax, write the Gherkin scenario for this test case.
### C. Test Steps
- 1: the method makepyfile(String) is called with string input with the format and text of the default file that can be ran with pytester.
- 2: runpytest() is called and the string output is stored in the variable result
- 3: finally the program does a check to see if result matches the expected string output. If it matches, it succeeds.
### D. Code Segments Under Test
- def test_runTest_method(pytester: Pytester) -> None:
    pytester.makepyfile(
        """
        import unittest
        class MyTestCaseWithRunTest(unittest.TestCase):
            def runTest(self):
                self.assertEqual('foo', 'foo')
        class MyTestCaseWithoutRunTest(unittest.TestCase):
            def runTest(self):
                self.assertEqual('foo', 'foo')
            def test_something(self):
                pass
        """
    )
    result = pytester.runpytest("-v")
    result.stdout.fnmatch_lines(
        """
        *MyTestCaseWithRunTest::runTest*
        *MyTestCaseWithoutRunTest::test_something*
        *2 passed*
    """
    )

  Notably the method makepyfile is used to create the default generic test, runpytest executes a test based on the generated test, and then stdout.fnmatch_lines() verifies the value of result

```Java
// Enter code
```
### E. Initial State
- The test doesn't have any preconditions in order to pass. There is no specific required state for this test.
### F. Transition
- After the test file is generated, runpytest is called and then soon after stdout.fnmatch_lines() is called. If the string matches or does not match, the system state updates to reflect if the test was successful or not.
### G. Expected State
- The expected state is for result to have this string stored within: """
        *MyTestCaseWithRunTest::runTest*
        *MyTestCaseWithoutRunTest::test_something*
        *2 passed*
    """
  

## III. Test Case 2: [def test_early_skip(self, pytester: Pytester) -> None:] in acceptance_test.py
### A. Description
- This test creates a makeconftest file, and then tests if skip() in the file forces the test to be skipped or not. To my understand a conftest file is ran before tests are conducted and can do things like load plugins and specify directories to be used
### B. Gherkin Syntax (if applicable)
- If you choose to use Gherkin syntax, write the Gherkin scenario for this test case.
### C. Test Steps
- 1: mkdir(String) is called with input "xyz" to create new directory xyz
- 2: makeconftest(String) is called to make a conftest file
- 3: the confest is ran with runpytest
- 4: in the conftest file there is a flag set with skip("early" that forces the test to be skipped
- 5: To check if it worked, stdout.fnmatch_lines(["*1 skip*"]) is called and compared to the output of the test in the variable result
- 6: If it matches, the test succeeds
### D. Code Segments Under Test
 pytester.mkdir("xyz")
        pytester.makeconftest(
            """
            import pytest
            def pytest_collect_file():
                pytest.skip("early")
        """
        )
        result = pytester.runpytest()
        assert result.ret == ExitCode.NO_TESTS_COLLECTED
        result.stdout.fnmatch_lines(["*1 skip*"])

        The method makeconftest(String) is used to make a conftest file, mkdir() makes a new directory, runpytest() runs conftest, and fnmatch_lines() checks the output
```Java
// Enter code
```
### E. Initial State
- The intitial state of the system must be before the series of pytests have been ran as the conftest sets up configurations for the pytests being ran
### F. Transition
- runpytest() is called and the output is returned to variable result. The variable is then compared to the expected output
### G. Expected State
- The expected output is "*1 skip*"

