

# Static Code Analysis using Sonar Qube Cloud
-----

#### Author:

Written by Akash Tyagi. You can reach me at: akashdktyagi@gmail.com

-----
####Details: 
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

![Image](Screenshot%202020-11-19%20at%207.16.32%20PM.png)

* Enter your Git Hub Credentials.
![Image](Screenshot%202020-11-19%20at%207.16.43%20PM.png)

* Click on (+) sign at the Top Right (beside search box) and click on Analyze new Project

![Image](Screenshot%202020-11-19%20at%207.16.56%20PM.png)

* Select Organization as your username, this will list all the repo under your username of Org

![Image](Screenshot%202020-11-19%20at%207.17.06%20PM.png)

* Few Projects like maven project, SonarCloud is not able to automatically analyze, so we need to make use of GitHub Actions to set up Sonar Qube for our Project.

![Image](Screenshot%202020-11-19%20at%209.46.49%20PM.png)

* Click on  "With GitHub Actions" link

![Image](Screenshot%202020-11-19%20at%207.18.23%20PM.png)

* Go to Settings > Secrets in your repository and add SONAR TOKEN in Secrets

![Image](Screenshot%202020-11-19%20at%209.55.42%20PM.png)

![Image](Screenshot%202020-11-19%20at%209.57.40%20PM.png)


