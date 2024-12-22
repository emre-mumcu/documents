# Set the JAVA_HOME Environment Variable

1. Open the Start Menu, search for "Environment Variables", and select Edit the system environment variables.
2. In the System Properties window, click on the Environment Variables button at the bottom.
3. Under System variables, click on New to add a new environment variable:
    Variable name: JAVA_HOME
    Variable value: The path to your JDK directory (e.g., C:\Program Files\Java\jdk-17.0.1).
4. Click OK to save the new variable.

# Add JAVA_HOME to the PATH Variable

1. In the Environment Variables window, under System variables, scroll down and find the Path variable. Select it and click Edit.
2. In the Edit Environment Variable window, click New and add the following:
    %JAVA_HOME%\bin
    This will add the bin directory of your JDK to the system's Path.
3. Click OK to close all the windows.

Type the following command to check if Java is correctly set up:
java -version