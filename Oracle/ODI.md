# Oracle Data Integrator Installation

Before you can install any Oracle Fusion Middleware product using a generic installer, you must download and install a supported JDK on your system before attempting to run the installer. You can then use the java -jar command to run the installer JAR file. Oracle Fusion Middleware products required `JDK 8.0 Update 101` or later. However, you should always verify the required JDK version by reviewing the certification information. When checking the certification, system requirements, and interoperability information, be sure to check specifically for any 32-bit or 64-bit system requirements. It is important for you to download software specifically designed for the 32-bit or 64-bit environment, explicitly. Check the certification using the following link:

https://www.oracle.com/middleware/technologies/fusion-certification.html

In the Oracle Fusion Middleware Supported System Configurations page, select `System Requirements and Supported Platforms for Oracle Fusion Middleware 12c (12.2.1.2.*)`and check the Excel file.

Also refer to Download, Installation, and Configuration Readme Files using the following link:

https://docs.oracle.com/cd/E23104_01/download_readme.htm

## 1. JDK Installation & Configuration

Open the Java SE 8 Archive Downloads (JDK 8u211 and later) using the following link and select the appropriate file for your system.

https://www.oracle.com/tr/java/technologies/javase/javase8u211-later-archive-downloads.html

Run the installer and finish the JDK setup. Check you java version and edit the PATH variable if required: 

```bat
java -version
set JAVA_HOME="C:\Program Files\Java\jdk1.8.0_221"
set path=C:\Program Files\Java\jdk1.8.0_221\bin;%path%
```

Or add the following environment variables (Windows)

```text
JAVA_HOME

# Set Variable name to JAVA_HOME
# Set Variable value to the path of your JDK (e.g., C:\Program Files\Java\jdk1.8.0_xx)

PATH

# Update the PATH Variable (optional but recommended)
# Find the Path variable in the System variables section, select it, and click Edit.
# Add a new entry: %JAVA_HOME%\bin
```

## 2. ODI Installation

First you have to download the ODI using following link:

https://www.oracle.com/tr/middleware/technologies/data-integrator-downloads.html

Please select the version:

* Oracle Data Integrator 11gR1 (11.1.1.9.0)

As of writing of this document, 12c versions of Oracle Data Integrator 12cR2 (12.2.1.2.6) / Oracle Data Integrator 12c (12.2.1.4.0) had some problems with creating master repositories.

For 11g: Double click and run the appropriate setup file located in `11.1.1.9.0\Disk1\install\` folder. (11.1.1.9.0\Disk1\install\win64\setup.exe)

For 12c: Start the installation using below command

``` bat
java -jar fmw_12.2.1.4.0_odi.jar
```

**After the installation completes, copy `msvcr100.dll` file from `Windows\System32` to `JAVA_HOME\jre\bin` folder.**

### ODI 12c UI Problem Resolution

After installing ODI 12c, when you start the application if the UI is irresponsive or laggy, upadte the `odi.conf` file in the `C:\Oracle\Middleware\Oracle_Home\odi\studio\bin` folder and add one of the following AddVMOption options:

``` batch
# set JAVA_HOME=C:\java\jdk8
# AddVMOption -Dswing.defaultlaf=com.sun.java.swing.plaf.windows.WindowsClassicLookAndFeel
# AddVMOption -Dswing.defaultlaf=javax.swing.plaf.metal.MetalLookAndFeel
AddVMOption -Dswing.defaultlaf=javax.swing.plaf.nimbus.NimbusLookAndFeel
# AddVMOption -Dswing.defaultlaf=com.sun.java.swing.plaf.motif.MotifLookAndFeel
# AddVMOption -Dswing.defaultlaf=com.sun.java.swing.plaf.windows.WindowsLookAndFeel
```

In order to list all availavle look and feels in your system run the following java program:

```java
// LookAndFeels.java
import javax.swing.*;

public class LookAndFeels 
{
    public static void main(String[] args) {
        UIManager.LookAndFeelInfo[] looks = UIManager.getInstalledLookAndFeels();

        for (UIManager.LookAndFeelInfo look : looks) {
            System.out.println("Name: " + look.getName() + ", Class: " + look.getClassName());
        }
    }
}

// % javac LookAndFeels.java
// # This command creates LookAndFeels.class. To run the program:
// % java LookAndFeels
```

# 3. Master & Work Repository Creations

https://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/odi/10g/10135/odimaster_work_repos/odimaster_work_repos.htm

A relational schema must be created for each repository. The following steps walk you through creating the relational schema for the ODI Master repository.

## 3.1. Creating the RDBMS Schema/User (Oracle 10g XE) for the Master Repository

https://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/odi/10g/10135/odimaster_work_repos/odimaster_work_repos.htm#e1

**NOTE**

For Oracle database 12c and later, you need to connect PDB NOT CDB using the service definition of PDB (127.0.0.1:1521/XEPDB1).

```sql
-- Create the schemas by executing the following SQL commands:
create user snpm1 identified by password default tablespace users temporary tablespace temp;
-- Grant connect privileges to the newly created user by executing the following SQL command:
grant connect, resource to snpm1;
-- Gice the user unlimited quota
alter user snpm1 quota UNLIMITED on USERS;
```

## 3.2. Creating the ODI Master Repository

To create the ODI Master repository, open the ODI and select File-> New and select Create a New Master Repository. 

In the URL field, enter the URL `jdbc:oracle:thin@localhost:1521:XEPDB1`, and then enter the User as `snpm1` and Password as `password`. 

Database Connection
---------------------------------------------------------
Technology	: Oracle
JDBC Driver	: oracle.jdbc.OracleDriver
JDBC URL	: jdbc:oracle:thin:@127.0.0.1:1521/XEPDB1
User		: SNPM1
Password	: password
DBA User	: system
DBA Password	: aA123456

Authentication
---------------------------------------------------------
Use ODI Authentication
Supervisor User     : SUPERVISOR
Supervisor Password : aA123456

Password Storage
---------------------------------------------------------
Internal Password Storage

## 3.3. Connecting to the ODI Master Repository

To connect to the ODI Master repository, open ODI and start Topology Manager. 

Click the New icon to create a new connection to the Master repository. Configure Repository Connections with the parameters provided in the following table. Click the Test button. Verify successful connection and click OK. Click OK to save the connection.

Oracle Data Integrator Connection
---------------------------------------------------------
Parameter	        Value
Login Name	        Master Repository
User	            SUPERVISOR
Password	        aA123456
Database Connection (Master Repository)
Parameter	        Value
User	            snpm1
Password	        password
Driver List	        Oracle JDBC Driver
Driver Name	        oracle.jdbc.driver.OracleDriver
Url	                jdbc:oracle:thin:@127.0.0.1:1521/XEPDB1

Select the newly created repository connection (Master Repository) from the drop-down list. Click OK. The ODI Topology Manager starts.

## 3.4. Creating and Connecting to the ODI Work Repository

```sql
-- Create the schemas by executing the following SQL commands:
create user snpw1 identified by password default tablespace users temporary tablespace temp;
-- Grant connect privileges to the newly created user by executing the following SQL command:
grant connect, resource to snpw1;
-- Gice the user unlimited quota
alter user snpw1 quota UNLIMITED on USERS;
```

Open Oracle Data Integrator and select Topology Manager tab. Click Connect To Repository and choose the newly created Master repository. Click OK.

Clik Repositories tab and right click Work Repositories and select New Work Repository

Enter the following parameters:
---------------------------------------------------------
Parameter	    Value
Name	        WORKREP
Technology	    Oracle
User	        snpw1
Password	    password

NOTE: Leave WORKREP1 password blank

Open ODI designer to connect work repository