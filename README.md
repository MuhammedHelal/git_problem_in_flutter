Processed **Only** If you have git installed and added successfully to your path but Flutter says unable to find git in path

💡 The Problem
When you install Git after Flutter, the Flutter setup script sometimes fails to correctly locate Git, leading to the infamous error:

Error: Unable to find git in your PATH.

✨ The Modern Solution
This problem can be resolved by modifying a single line in a Flutter internal batch file to manually bypass the Git existence check.

Step 1: Locate the File
You need to edit the file located at:

"flutter-dir"/bin/internal/shared.bat
Replace "flutter-dir" with the actual path to your Flutter installation.

Step 2: Edit the File
Open the shared.bat file and search for the section related to git_exists and 2>NUL.

You will modify this section to force git_exists to true and comment out the block of code that checks for Git's existence and revision.

The lines that need to be changed/commented are:

Code snippet

 REM Check that git exists and get the revision  
 SET git_exists=true  
 @REM 2>NUL (  
 @REM    PUSHD "%flutter_root%"  
 @REM    FOR /f %%r IN ('git rev-parse HEAD') DO (  
 @REM       SET git_exists=true  
 @REM       SET revision=%%r  
 @REM    )  
 @REM    POPD  
 @REM )  
 REM If git didn't execute we don't have git. Exit without /B to avoid retrying.  
 if %git_exists% == false echo Error: Unable to find git in your PATH. && EXIT 1  
 SET compilekey="%revision%:%FLUTTER_TOOL_ARGS%"

 
✅ What Changed?
SET git_exists=true: This line is moved to the top of the block and manually sets the variable to true, bypassing the check.

@REM ...: The entire block of code that performs the git rev-parse HEAD check and sets git_exists is commented out using @REM.

@REM 2>NUL (: The 2>NUL (which suppresses errors) is commented out as the check is now bypassed.

🥳 Problem Solved!
After saving the modified shared.bat file, the Flutter commands should execute without the "Unable to find git in your PATH" error.
