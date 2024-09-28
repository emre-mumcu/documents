# ODI 12C Installation

1) First you have to download the latest ODI using below link.

https://www.oracle.com/tr/middleware/technologies/data-integrator-downloads.html

2) Check you java version & set the java variables.

```bat
java -version
set JAVA_HOME="C:\Program Files\Java\jdk1.8.0_221"
set path=C:\Program Files\Java\jdk1.8.0_221\bin;%path%
```

Or add the following environment variables in Windows.

```
JAVA_HOME
Set Variable name to JAVA_HOME
Set Variable value to the path of your JDK (e.g., C:\Program Files\Java\jdk1.8.0_xx)

Update the PATH Variable (optional but recommended)
Find the Path variable in the System variables section, select it, and click Edit.
Add a new entry: %JAVA_HOME%\bin
```

3) Start the installation using below command

``` bat
java -jar fmw_12.2.1.4.0_odi.jar
```

After installation completes, copy msvcr100.dll file from Windows\System32 to JAVA_HOME\jre\bin folder.

# ODI UI Problem Resolution

After installing ODI, when you start the application if the UI is irresponsive or laggy, upadte the `odi.conf` file in the `C:\Oracle\Middleware\Oracle_Home\odi\studio\bin` folder and add the following options:

``` batch
set JAVA_HOME=C:\java\jdk8
AddVMOption -Dswing.defaultlaf=com.sun.java.swing.plaf.windows.WindowsClassicLookAndFeel
AddVMOption -Dswing.defaultlaf=javax.swing.plaf.metal.MetalLookAndFeel
AddVMOption -Dswing.defaultlaf=javax.swing.plaf.nimbus.NimbusLookAndFeel
```

In order to list all availavle look and feels run the following java program:


```java
// LookAndFeelDemo.java
import javax.swing.*;

public class LookAndFeelDemo 
{
    public static void main(String[] args) {
        UIManager.LookAndFeelInfo[] looks = UIManager.getInstalledLookAndFeels();

        for (UIManager.LookAndFeelInfo look : looks) {
            System.out.println("Name: " + look.getName() + ", Class: " + look.getClassName());
        }
    }
}

// % javac LookAndFeelDemo.java
// # This command creates LookAndFeelDemo.class, to run the program:
// % java LookAndFeelDemo
```

# Master & Work Repository Setup

## Create User

```sql
-- User Creation
CREATE USER <schema_name> IDENTIFIED BY <password>;
GRANT CONNECT, RESOURCE TO <schema_name>; 
GRANT CREATE SESSION TO <schema_name> ;
GRANT DBA TO <schema_name> ;
GRANT ALL PRIVILEGES TO <schema_name>;

-- ODI_PEPO
-- If CDB:
ALTER SESSION SET CONTAINER = XEPDB1;

CREATE USER ODI_REPO IDENTIFIED BY welcome;
GRANT CONNECT, RESOURCE, DBA TO ODI_REPO;
GRANT CREATE SESSION TO ODI_REPO;
GRANT DBA TO ODI_REPO;
GRANT ALL PRIVILEGES TO ODI_REPO;

-- Meta Queries
SELECT SYS_CONTEXT('USERENV','SESSION_USER') from DUAL;
SELECT SYS_CONTEXT('USERENV','CURRENT_SCHEMA') FROM DUAL;
SELECT SYS_CONTEXT('USERENV','IP_ADDRESS') FROM dual;
SELECT SYS_CONTEXT('USERENV','HOST') FROM dual;

select * from V$NLS_PARAMETERS;
SELECT * FROM V$SERVICES;
SELECT * FROM V$PARAMETER;
SELECT * FROM V$SESSION;
SELECT * FROM V$INSTANCE;
SELECT * FROM V$DATABASE;
```

## Setup Master Repository

Open the ODI 12c Studio. Go to File -> New. Select "Create  New Master Repository".

Provide all the details for repository connection:

```
Technology      : Oracle
JDBC Driver     : oracle.jdbc.OracleDriver
JDBC URL        : jdbc:oracle:thin:@127.0.0.1:1522:XE
JDBC URL        : jdbc:oracle:thin:@127.0.0.1:1522/XEPDB1
User            : ODI_REPO
Password        : pwd
DBA User        : system
DBA Password    : pwd
```

After providing all the details, test the connection. Click next.

Provide authentication mode using ODI authentication, provide supervisor password & note it down somewhere as it is required for login setup.

```
Use ODI Authentication
----------------------
Supervisor User         : SUPERVISOR
Supervisor Password     : pwd
Confirm Password        : pwd
```

Click on finish after providing supervisor password. It will start the setup of the master repository. It will take few minutes to complete the setup of master repository.

## Setup Master Repository Login

Open the ODI 12c Studio. Go to File -> New. Select "Create  New ODI Repository Login".

Provide the details you provided at the time of repository setup. For Database Connection, provide details for master reposotory schema details. Select radio button master repository. Test the connection.

```
Oracle Data Integrator Connection
---------------------------------
Login Name      : MASTER_LOGIN
User            : SUPERVISOR
Password        : pwd

Database Connection
-------------------
User            : ODI_REPO
Password        : pwd
Driver List     : Oracle JDBC Driver
Dirver Name     : oracle.jdbc.OracleDriver
URL             : jdbc:oracle:thin:@127.0.0.1:1522:XE

Work Repository
---------------
* Master Repository Only
```

Click on ‘Ok’. You are done with the master repository login setup. Now you have to login your master repository & create the work repository inside it.

## Login to Master Repository

In ODI 12c Studio. Click on connect repository. Select the master repository you have created and connect.

Now you have to setup work repository under this master repository.

## Setup Work Repository

Go to Topology Navigator, Repositories -> Work Respoitories -> Right click and select "New Work Repository".

```
Technology      : Oracle
JDBC Driver     : oracle.jdbc.OracleDriver
JDBC URL        : jdbc:oracle:thin:@127.0.0.1:1522:XE
User            : ODI_REPO
Password        : pwd
```

Click OK.

Here the work repository setup &  login has been done & disconnect from master repository and login the work repository.

NOTE:

If you encounter "Warning - could not install some modules" while opening the ODI 12c Sudio; delete all the temp files present in the below directory & try opening the ODI studio again.

```
C:\Users\<<Username>>\AppData\Roaming\odi\system12.2.1.4.0
```