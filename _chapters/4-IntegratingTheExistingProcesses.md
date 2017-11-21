---
title: Integrating the Existing Processes
---

## 4.1. Start with the existing report

Writing tests is part of the overall development cycle, and therefore 
begins with a good feature request or bug report:


**User Issue Report**
<div style="border: 1px solid black;">
 <b>Title: Suggested text is not a link (fp12345)</b>
 
 When performing a search on the website, the search 
 engine identifies suspected typos and suggests an 
 alternate search.
 
 1. GOTO: http://example.ca/search.html
 2. Enter: webteam
 3. Click: Search
 
 <dl>
  <dt>Expected</dt>
   <dd>The suggested spelling "Web Team" should be a link to perform that search</dd>
  <dt>Actual</dt>
   <dd>The text is present, but it is not a link</dd>
 </dl>
</div>

The key to this bug report is that it contains explicit steps that 
are able to reproduce the error (the basics of a good bug report), 
and steps to reproduce an error are a test of the system that should 
be formalized.

While this report is very clear as to <em>what</em> is going on, it 
still remains for a developer to determine why it is happening. After 
a technical Analysis, the Issue needs to be redefined taking into 
account the technical information:

**Bug Description Report**
<div style="border: 1px solid black;">
 <b>Title: "Did you mean" text should be a link (rm54321)</b>

 <table>
  <tbody>
   <tr> 
    <th>Related</th>
    <td>FP12345</td>
   </tr>
  </tbody>
 </table>

 When performing a search on the website, the search engine 
 identifies suspected typos and suggests an alternate search. In the 
 case that the suggested text is multiple words, a link is generated 
 with a space in the url:
 
 <blockquote>http://example.ca/search.html?q=web team</blockquote>

 This is an invalid URL and should read:

 <blockquote>http://example.ca/search.html?q=web+team</blockquote>

 Because the link is invalid, the web engine's link checking routines 
 determine it to be an invalid URL and does not apply it.

 <b>Test</b>
 <table>
  <tbody>
   <tr>    <td>1</td>    <td>GOTO</td>    <td>http://example.ca/search.html?q=webteam</td>   </tr>
  </tbody>
 </table>
 <dl>
  <dt>Expected</dt>
   <dd>The suggested spelling “Web Team” should be a valid link</dd>
 </dl>
</div>

Drawing directly from the user's issue report, we are able to clearly 
define the problem in technical terms as well as generate a simpler, 
and more reliable, test to determine that we have corrected the 
problem. Using this definition of a successful test, we can begin 
creating an automated Unit Test.

```java
package ca.company.web.test.pages;

public class Search extends BaseTest {

  @Test(enabled=false,testName="rm1242")
  public void testSearchSpellCheck() {
  }

}
```

The basic construction of this is based on several conventions in our 
system:

The package and classname are the full path the webpage we are 
testing. In the case of a component it would be the path to the 
component. In this case, the page

> `/content/example/en/home/search`

corresponds to

> `ca.company.web.test.pages.Search`

It is also worth noting that the `TestClass` inherits from 
`BaseClass`, which is an internal class created specifically for the 
Company that contains several routines useful to testing at the 
Company in general.

The decorator `Test` has been used to pass messages about the test to 
the TestNG engine.

<dl>
 <dt>@Test</dt>
  <dd>
   the presence of the @Test marker tells the engine that this is a 
   test
  </dd>
 <dt>testName=rm1242</dt>
  <dd>
   gives a specific name to the test. This allows us to identify it 
   in relation to the bug that identified it.
  </dd>
</dl>

There are several other useful parameters that can be applied. It is 
recommended that you read the TestNG Documentation for more 
information.

By convention, all test functions start with the word “test” in 
lowercase. This allows us to group test routines separately from 
administrative routines in the same class. Also tests are the only 
functions that are to be public. While TestNG is not actually 
affected by this, for documentation purposes we can allow JavaDoc to 
pick up on all public functions, and know that it is only getting 
tests.

Once we have the test in place, the next step is to document the 
test. In an effort to keep all of our testing notes and regression 
test listing in one place, we use JavaDoc’s capabilities to put 
documentation and code in the same place. This means we can document 
our tests as well as maintain the code in conjunction with one 
another.

Currently, the eclipse project is the repository for all test cases.

The test definition in the comments should be a well formatted HTML 
document, describing the test to be performed. It should include a 
title (H1), description (paragraphs) and a detailed list outlining 
the steps to be performed. There should always be a standardized 
format for these test descriptions, this helps ensure they are always 
completely filled out, increases the rapidity which new people can 
follow them, and allows experienced individuals to retrieve 
information at a glance.

```java
package ca.company.web.test.pages;


/**
 * Suite of tests for the Search Page
 * &lt;p&gt;
 * The Search page is one of the prime navigation points for company’s system
 * &lt;/p&gt;
 * &lt;pre&gt;
 * $Id$
 * $URL$
 * &lt;/pre&gt;
 */
public class Search extends BaseTest {

  /**
   * &lt;h1&gt;rm1242: "Did you mean" text should be a link&lt;/h1&gt;
   * &lt;p&gt;
   * When performing a search on the website, the search engine 
   * identifies suspected typos and suggests an alternate search. 
   * In the case that the suggested text is multiple words, a link
   * is generated with a space in the url:
   * &lt;blockquote&gt;&lt;pre&gt;http://example.ca/search.html?q=web team&lt;/pre&gt;&lt;/blockquote&gt;
   * &lt;/p&gt;
   * &lt;p&gt;
   * This is an invalid URL and should read:
   * &lt;blockquote&gt;&lt;pre&gt;http://example.ca/search.html?q=web+team&lt;/pre&gt;&lt;/blockquote&gt;
   * &lt;/p&gt;
   * &lt;p&gt;
   * Because the link is invalid, the search engine's Link Checker determines it 
   * to be an invalid URL and does not apply it.
   * &lt;/p&gt;
   * &lt;p&gt;
   * Procedure:
   * &lt;ol&gt;
   * &lt;li&gt;GOTO: http://example.ca/search.html?q=webteam&lt;/li&gt;
   * &lt;/ol&gt;
   * RESULT: 
   * &lt;ul&gt;
   *  &lt;li&gt;search suggests "Did you mean: web team"&lt;/li&gt;
   *  &lt;li&gt;"web team" should be a link&lt;/li&gt;
   * &lt;/ul&gt;
   * &lt;/p&gt;
   * 
   */
  @Test(testName="rm1242")
  public void testSearchSpellCheck() {
    throw new ManualTestException();
  }
}
```

In the above example, note the throwing of “ManualTestException”. 
This Exception inherits from TestNG’s “SkipException”. It tells the 
test engine not to disable the test, but to report that the test must 
be run manually. From a Documentation point of view, this test 
definition is complete. A manual tester can pick up the JavaDocs and 
see that a test must be undertaken to verify system validity.

At this point, running the test will return a “yellow” indicator, and 
state that the test must be run manually. If you look at the JavaDoc 
view in Eclipse, the steps to manually verify this will be laid out.


## 4.2 Test Runs

Performing a Test Run is as simple as running the pertinent automated 
tests from the test project, and following up with any non-passes 
that may occur. 

“Following up” would be a matter of watching for anything that is not 
a “Pass”. This could be either a fail, or an Manual Test. The 
automated tests offer a way to eliminate sections of testing rapidly 
(no further work is required on automated passes). Basically, the 
tests should offer suggestions for further action

That’s it. You’re done.


## 4.3. Automated Tools

One of the first things I do when coming to any company is to 
implement an automated test framework for the consistent development 
of Unit Tests. 

#### CompanyTests Project

The project should be self-contained to allow for maximum portability 
between all developers, testers, and anyone else we can convince to 
write tests. That means it should be a simple matter of checking the 
project out of SVN and opening it with Eclipse.

### Sample Test Run

The simplest way to verify that your environment is configured 
correctly is to run a couple of tests. There are two types of tests 
stored in the project: system scans, and unit tests. Since the 
purpose differs, the way they will typically be used is different. It 
is a good idea to run through each of the two types of runs.


#### Sample 1: Run smoke tests

Tests can be grouped through the use of XML configuration files that 
select specific tests to run, and pass parameters into the system.

The Smoke Tests push information into various parts of the system and 
check for problems. They are not a detailed check but scan large 
portions of the system.

1. Open: CompanyTests > suites > smoke.xml
2. Check configuration parameters
   * SampleSize: 1.1
   * BaseURL: http://www-staging.cms.example.ca
3. In Package Explorer (left hand menu), right click on “smoke.xml”
4. Select “Debug As” > TestNG Suite

In the console you should see “[TestNG] Running:”. Next to the 
console tab, there is a “Results of running suite”, this should have 
a list of tests that have been run, are running, and have failed.


### Project Layout

There are four main sections to the test packages:

| Components | ca.company.web.tests.components.* |
| Templates | ca.company.web.tests.templates.* |
| System | ca.company.web.tests.system.* |
| Utilities | ca.company.web.tests.* |

Each of these represents a “testable” area, with the exception of the 
Utilities which are helper classes used to manage the tests in 
general. 

Within each of these areas, the project is laid out to mirror the 
development project. In particular, for each component path in Dev, 
there is a corresponding component path in CompanyTests. This allows 
a developer to easily switch between the two code bases.

### Writing Tests

Once a test has been documented, there is certainly an opportunity to 
automate this test. Automation is faster and more reliable, so we 
should attempt to automate every test.

Before writing the actual test steps, it is necessary to get a 
browser instance. Locally, we have a helper routine for getting 
browser instances (BrowserPool). BrowserPool and the drivers it 
returns are sensitive to setup and teardown requirements and will 
take care of much of the setup and releasing of the browser 
instances.

It is also best to set up any constants we are going to use right 
away. This is to match the pattern found in the documentation.

* search - the text we are going to search for
* suggest - the text we expect to get back
* pageUrl - the URL of the page we are going to use for testing.

```java
@Test(testName="rm1242")
public void testSearchSpellCheck() {
    final String search = "webteam";
    final String suggest = "web team";
    final String pageUrl = baseUrl + this.pageUrl + "?q=" + search + "&stype=main";
    
    try(BrowserPooledDriver pooleddriver = browsers.borrowObject()){
      WebDriver driver = pooleddriver.getDriver();
      // actual test steps go here
    }
}
```

At this point, we are ready to actually automate the test.

For this, the documentation offers excellent pseudocode. The 
step-by-step procedure has been tested by humans running it, and is 
very detailed. Really, it is code for than runs on a “MeatBag 
Processor Unit”. 


```java
try(BrowserPooledDriver pooleddriver = browsers.borrowObject()){
	WebDriver driver = pooleddriver.getDriver();
	// STEP 1: GOTO: http://example.ca/search.html?q=webteam
	// RESULT: search suggests "Did you mean: web team"</li>
	// RESULT: "web team" should be a link
}
```

At this point, I would strongly suggest running the code. This would 
be a really convenient time to find out that you have made a mistake 
in the administrative code. Much more convenient than after you have 
put a bunch of test code in place.

To run the code:

1. Find the method in the “Package Explorer” or “Outline” view of 
   Eclipse.
2. Right-click on the test
3. Select “Debug As” -> TestNG

You should see it kick in and run the test. Hopefully you get a green 
icon!

Now that we have the baseline in place, we can move on to actually 
automating the test steps. Our description can easily be translated 
into automated steps (all one of them), and the checks for results we 
are expecting.

```java
try(BrowserPooledDriver pooleddriver = browsers.borrowObject()){
	WebDriver driver = pooleddriver.getDriver();
	// STEP 1: GOTO: http://example.ca/search.html?q=webteam
	driver.get(pageUrl);
	
	// RESULT: search suggests "Did you mean: web team"</li>
	elem = driver.findElement(By.xpath("//div[@class='spelling']/a"));
	Assert.assertNotNull(elem, "Presence of new search link");

	// RESULT: "web team" should be a link
	elem = driver.findElement(By.xpath("//div[@class='spelling']/a"));
	Assert.assertEquals(elem,suggest,"Spelling suggestion made");
}
```

We use the Selenium Webdriver to operate a web browser instance. In 
our example above, we can see the driver does a “findElement” call, 
this looks up the webpage element (any HTML tag). For a parameter, 
you need to call one of the methods of the “By” class, which is how 
we address the element we are attempting to look up. In this case, we 
are attempting to find an “anchor” tag, by its xpath.

It is worth noting that Selenium is not the only way to gather data 
about our environment. In strict UnitTesting, we would be directly 
calling the methods on each class we write in our library. In the 
case we are checking header information, Selenium can’t help, we need 
to turn to other HTTP communication mechanisms. Perhaps we want to 
test a terminal application, we could use a command streamer to act 
as our interface.

Once we have obtained the element we can run some Assertions on it. 
Assertions are the falsifiable statements about the data we have 
gathered. TestNG gives us an Assert class that contains several 
assert methods that allow us to make different kinds of statements 
about the data. Every assertion should have a textual description 
associated with it. In the first case, we are going to assert that we 
found the element at all (did not get null); the second case checks 
to see that it contains the text we are anticipating.

Now would be a good time to run the test again.

Assuming you got a green-light from the test, it is worth taking a 
moment to look back on what we now have.

<dl>
 <dt>Automated Test</dt>
  <dd>
   obviously we have created an automated test. Faster and more reliable.
  </dd>
 <dt>Manual Test</dt>
  <dd>
   using JavaDoc, we are able to maintain steps for the test to be 
   performed and reviewed by non-technical individuals. If automation 
   were not available, or not possible, we still have a test plan.
  </dd>
 <dt>Regression Library</dt>
  <dd>
   we have a complete regression library. This library can be used to 
   review tests over time. When a major release is performed, we will 
   have a list of known issues that need to be checked against. 
   Stakeholders have a listing of everything that is being checked 
   and can highlight areas that are being overlooked. This also gives 
   the foundation for effort estimation as we can identify how long 
   tests take to run and compare that to the number of tests required 
   for a given run.
  </dd>
 <dt>Complete Specifications</dt>
  <dd>
   The tests in a regression library offer a complete description of 
   the specifications of the system. If the specifications need to 
   change, there is a source of information for understanding what 
   the original functionality was, and what ramifications may be 
   present.
  </dd>
</dl>


