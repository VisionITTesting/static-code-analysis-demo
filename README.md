# Static Code Analysis using Sonar Qube Cloud

---

#### Author:

Written by Akash Tyagi. You can reach me at: akashdktyagi@gmail.com

-----
#### Details: 
* Static code analysis is a very common practice among the developers. 
* Unfortunately, not very common among Automation Test developers. 
* Automated Static code analysis is an absolute necessity to any type of code. 
* It identifies a lot of coding mistakes which we do and are not very aware of. 
* There are many tools some are paid and some are open source for static code analysis. 
* You can use SonarQube Cloud Analysis for this purpose.

---
#### Steps to Set Up

* Create a Maven Project
* Add below dependency in the POM file:
```aidl
 <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>3.141.59</version>
        </dependency>
    </dependencies>
```
* Create package under ```test.java``` with name as ```testcases```
* Create a Test Class: ```test.java.testcases.RunTest```
* Add test case to invoke the browser and navigate to the amazon.in web site
* Note that ```@Test``` annotation is used to invoke the test cases. This annotation is provided to us by Junit library which we have added.
* Also Note annotation ```@Before``` ```@After```. 
* All the annotation which has ```@Before``` annotated method will execute before every test.
* All the annotation which has ```@After``` annotated method will execute after every test.

```aidl
package testclasses;

import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import java.util.concurrent.TimeUnit;

public class RunTest {

    WebDriver driver;
    String base_url = "https://amazon.in/";
    int implicit_wait_timeout_in_sec = 20;

    //This method will execute before every Test method
    //Since, we need to invoke Browser for every test case, we will use this annotation to have driver init steps
    @Before
    public void set_up(){
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(implicit_wait_timeout_in_sec, TimeUnit.SECONDS);
    }

    @Test
    public void t_01_check_website_is_working(){
        driver.get(base_url);
        String expected = "amazon";
        String actual =driver.getTitle();
        Assert.assertEquals("Page Title validation",expected,actual);
    }

    //This method will execute after the end of each @Test annotated method.
    @After
    public void clean_up(){
        driver.quit();
    }

}
```

---
#### Configure SonarQube Project
* Navigate to: [https://sonarcloud.io/](https://sonarcloud.io/)
* Click on Git Hub link

>![Image](Screenshot%202020-11-19%20at%207.16.32%20PM.png)

* Enter your Git Hub Credentials.
>![Image](Screenshot%202020-11-19%20at%207.16.43%20PM.png)

* Click on (+) sign at the Top Right (beside search box) and click on Analyze new Project

>![Image](Screenshot%202020-11-19%20at%207.16.56%20PM.png)

* Select Organization as your username, this will list all the repo under your username of Org

>![Image](Screenshot%202020-11-19%20at%207.17.06%20PM.png)

* Few Projects like maven project, SonarCloud is not able to automatically analyze, so we need to make use of GitHub Actions to set up Sonar Qube for our Project.

>![Image](Screenshot%202020-11-19%20at%209.46.49%20PM.png)

* Click on  "With GitHub Actions" link

>![Image](Screenshot%202020-11-19%20at%207.18.23%20PM.png)

* Go to Settings > Secrets in your repository and add SONAR_TOKEN variable in Secrets

>![Image](Screenshot%202020-11-19%20at%209.55.42%20PM.png)

>![Image](Screenshot%202020-11-19%20at%209.57.40%20PM.png)

* Add below lines in your POM.XML file.

>![Image](Screenshot%202020-11-19%20at%2010.33.13%20PM.png)

* Add Git Hub Actions yml file in your repo. 
>* Git Hub actions yml file is a specification file which is used to declaratively specify the CI-CD workflow for your repo. In this case we are using it for static analysis for each code push and PR on your ```master``` branch.
>* For more on how to implement Git Hub Actions, refer my other tutorial here: [CI-CD-Impl-Using-GitHubActions](https://visionittesting.github.io/ci-github-actions-demo/)
>* Git Hub Actions Tutorials can be found here: [GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions)

* Click on ```Actions``` tab in your Git Hub Repo and click on set up Java Maven workflow.

> ![Image](Screenshot%202020-11-20%20at%209.26.31%20AM.png)

* Above step will Create folder and file, ```.github/workflows/maven.yml``` in your repo.
> ![Image](Screenshot%202020-11-20%20at%209.28.14%20AM.png)

* Copy and paste below code in the Web Editor.
> ![Image](Screenshot%202020-11-20%20at%209.32.02%20AM.png)

> ![Image](Screenshot%202020-11-19%20at%2010.38.57%20PM.png)

* Text Version of the above screen shot. Make sure you copy this code from the actual Sonar Qube generated code mentioned in above steps, where you would configure Sonar Qube Project.
(However it is going to be exactly the same, so you can below yml spec as well)
```aidl
name: Build
on:
  push:
    branches:
      - master
  pull_request:
    types: [opened, synchronize, reopened]
jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0  # Shallow clones should be disabled for a better relevancy of analysis
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 11
      - name: Cache SonarCloud packages
        uses: actions/cache@v1
        with:
          path: ~/.sonar/cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
      - name: Cache Maven packages
        uses: actions/cache@v1
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
          restore-keys: ${{ runner.os }}-m2
      - name: Build and analyze
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Needed to get PR information, if any
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: mvn -B verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
```

* Once you have completed this



