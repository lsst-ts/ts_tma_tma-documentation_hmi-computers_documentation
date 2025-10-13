# User Management

This section explains how the user management works. The user management for the HMI is done using a commercial toolkit from WireFlow.

Using the Wireflow toolkit for user management, you must do the following:

1. Create the configuration file with the desired users. To do this, create the VI: ConfigUsersFile (within the path: `HMI Computers\HMI\Subsystem\Log In\User Management Tools`).
Running this VI will open a pop-up window of the Wireflow application from which users will be added/removed.
It also allows you to modify the user level, up to a limit of 5, as well as assign special characteristics to each level.
Once this is done, click File > Save.

2. The generated file is saved by default with the name: **HMI_UserManagementFile.uat**.
This is done because the HMI then reads this file and applies the configuration contained therein.

3. Once the users have been defined, the HMI application can be used.
As mentioned above, it reads the configuration file generated in step 1.

4. Regarding how the log-in works, the following steps apply:
  
    a. First, when running the HMI for the first time, only the user can log in. To do so, the following fields must be filled out:

        i. User: with a user from those defined in the pop-up window in step 1.

        ii. Password: Enter the password corresponding to the desired user.

        iii. Once filled out, click Log In. If either of the two fields is incorrect, a pop-up window will appear warning the user that either the user name or password is incorrect.
  
    b. Once this is done, the user is inside the app and can navigate and do whatever they want. However, access must be limited in the relevant windows based on user level.

    c. If you click the "Sign In as a different user" button from the home window, the password and sign-in controls will be disabled, allowing you to log out only. This means that the current user's name will appear in the user field, and only the sign-out button will be enabled.
  
    d. When you click "Sign Out," the sign-out button is disabled and the sign-in button is enabled. This is again the same as in point a. The only exception is that the name of the last user, the one who just logged out, will remain unchanged.
