# Client-Autometion-Android

# Final Setup and Implementation for 100ms App Automation Assignment (Windows OS with VS Code)

This guide will walk you through setting up the environment on **Windows** using **VS Code** and completing the **100ms App Automation** assignment using **Appium**. Follow each step carefully to ensure a successful setup.

---

## Step 1: Install Java JDK

1. **Download and Install Java JDK**:
   - Go to the [Java JDK Download Page](https://www.oracle.com/java/technologies/javase-downloads.html) and install the latest version of the **Java Development Kit (JDK)**.
   
2. **Set JAVA_HOME Environment Variable**:
   - Open **System Properties** → **Advanced** → **Environment Variables**.
   - Under **System variables**, click **New** and set:
     - Variable name: `JAVA_HOME`
     - Variable value: Path to your JDK installation (e.g., `C:\Program Files\Java\jdk-17.0.X`)
   - Add `%JAVA_HOME%\bin` to the **Path** in the system variables.

3. **Verify Installation**:
   - Open **Command Prompt** and type:
     ```bash
     java -version
     ```
   - You should see the installed JDK version.

---

## Step 2: Install Android SDK Command-Line Tools

1. **Download Android Command-Line Tools**:
   - Download **Command-Line Tools** from [here](https://developer.android.com/studio#downloads) under the section "Command line tools only".

2. **Extract SDK Tools**:
   - Extract the downloaded file to a directory like `C:\Android`.

3. **Set Environment Variables**:
   - Set `ANDROID_HOME = C:\Android` in **Environment Variables**.
   - Add the following paths to the **Path**:
     - `C:\Android\cmdline-tools\latest\bin`
     - `C:\Android\platform-tools`
     - `C:\Android\emulator`

4. **Install Necessary SDK Components**:
   - Open **Command Prompt** and run:
     ```bash
     sdkmanager --list
     sdkmanager "platform-tools" "platforms;android-30" "emulator"
     ```

---

## Step 3: Install Node.js and Appium

1. **Install Node.js**:
   - Download and install [Node.js](https://nodejs.org/).

2. **Install Appium**:
   - Open **Command Prompt** and install Appium globally:
     ```bash
     npm install -g appium
     ```

3. **Install Appium Doctor**:
   - To verify your environment, install Appium Doctor:
     ```bash
     npm install -g appium-doctor
     appium-doctor --android
     ```
   - Ensure all checks pass for Android automation.

4. **Start Appium Server**:
   - In the terminal, start the Appium server:
     ```bash
     appium
     ```

---

## Step 4: Set Up VS Code

1. **Install VS Code**:
   - Download and install [Visual Studio Code](https://code.visualstudio.com/).

2. **Install Required Extensions**:
   - Install the **Java Extension Pack** and **Appium Tools** extensions from the VS Code marketplace.

---

## Step 5: Install Appium Java Client Using Maven

1. **Create a Maven Project**:
   - Open **VS Code**, go to the terminal, and run:
     ```bash
     mvn archetype:generate -DgroupId=com.100ms.test -DartifactId=AppiumAutomation -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
     ```

2. **Update `pom.xml` with Dependencies**:
   - Add the **Appium Java Client** and **Selenium** dependencies in the `pom.xml` file:
     ```xml
     <dependencies>
         <dependency>
             <groupId>io.appium</groupId>
             <artifactId>java-client</artifactId>
             <version>8.0.0</version>
         </dependency>
         <dependency>
             <groupId>org.seleniumhq.selenium</groupId>
             <artifactId>selenium-java</artifactId>
             <version>4.0.0</version>
         </dependency>
     </dependencies>
     ```

3. **Build the Maven Project**:
   - In VS Code, open the terminal and run:
     ```bash
     mvn clean install
     ```

---

## Step 6: Configure Android Emulator or Real Device

1. **For Emulator**:
   - Create an Android emulator:
     ```bash
     avdmanager create avd -n Pixel_3 -k "system-images;android-30;google_apis;x86"
     ```
   - Start the emulator:
     ```bash
     emulator -avd Pixel_3
     ```

2. **For Real Device**:
   - Enable **Developer Mode** and **USB Debugging** on your device.
   - Connect your device and verify the connection:
     ```bash
     adb devices
     ```

---

## Step 7: Complete Automation Code

Here’s the Java code to automate the 100ms app as per the assignment requirements:

```java
package com.100ms.test;

import io.appium.java_client.AppiumDriver;
import io.appium.java_client.MobileBy;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.remote.MobileCapabilityType;
import org.openqa.selenium.remote.DesiredCapabilities;
import java.net.URL;
import java.util.concurrent.TimeUnit;

public class AppAutomation {
    public static void main(String[] args) {
        try {
            // Set desired capabilities for Appium
            DesiredCapabilities capabilities = new DesiredCapabilities();
            capabilities.setCapability(MobileCapabilityType.PLATFORM_NAME, "Android");
            capabilities.setCapability(MobileCapabilityType.PLATFORM_VERSION, "11.0");
            capabilities.setCapability(MobileCapabilityType.DEVICE_NAME, "Pixel_3");
            capabilities.setCapability(MobileCapabilityType.APP, "path/to/100ms/app.apk");
            capabilities.setCapability(MobileCapabilityType.AUTOMATION_NAME, "UiAutomator2");

            // Initialize Appium driver
            AppiumDriver driver = new AndroidDriver(new URL("http://localhost:4723/wd/hub"), capabilities);
            driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

            // Step 1: Automate the UI from start -> preview -> meeting page
            driver.findElement(MobileBy.id("com.package.name:id/startButton")).click();
            driver.findElement(MobileBy.id("com.package.name:id/previewButton")).click();
            driver.findElement(MobileBy.id("com.package.name:id/meetingButton")).click();

            // Step 2: How to get token details to run the App
            String token = driver.findElement(MobileBy.id("com.package.name:id/tokenField")).getText();
            System.out.println("Token: " + token);

            // Step 3: Automate one end-to-end flow of app launch and verify if the video shows up
            boolean isVideoDisplayed = driver.findElement(MobileBy.id("com.package.name:id/videoView")).isDisplayed();
            if (isVideoDisplayed) {
                System.out.println("Video stream is displayed successfully.");
            } else {
                System.out.println("Failed to display video stream.");
            }

            // Close the driver
            driver.quit();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Step 8: Running the Code

1. **Run Your Tests**:
   - Open the Maven project in **VS Code** and run the `AppAutomation.java` file using **F5** or through the VS Code terminal:
     ```bash
     mvn test
     ```

2. **Record the Automation**:
   - Record a video showing the script executing on an emulator or real device.
   - Use screen recording software like **OBS Studio** or Windows' built-in recording feature (`Windows + G`).

---

## Step 9: Submit the Assignment

- Make sure the automation code runs correctly.
- Submit the runnable code and the video demonstrating the automation process.