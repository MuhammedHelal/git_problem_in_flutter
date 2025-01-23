# git_problem_in_flutter
![image](https://github.com/user-attachments/assets/f3c3d655-ab42-4177-894c-5587bd81eef2)

If you installed git after flutter you will encounter unable to find git in your path error.
It`s a headace because there is no solution will work with you because Modern problems require modern solutions 


Edit  file "flutter-dir"/bin/internal/shared.bat"

search for git_exists, make it true and comment out 2>Nul


Now new file will looks like :

 REM Check that git exists and get the revision  
  SET git_exists=true  
  @REM 2>NUL (  
  @REM   PUSHD "%flutter_root%"  
  @REM   FOR /f %%r IN ('git rev-parse HEAD') DO (  
  @REM     SET git_exists=true  
  @REM     SET revision=%%r  
  @REM   )  
  @REM   POPD  
  @REM )  
  REM If git didn't execute we don't have git. Exit without /B to avoid retrying.  
  if %git_exists% == false echo Error: Unable to find git in your PATH. && EXIT 1  
  SET compilekey="%revision%:%FLUTTER_TOOL_ARGS%"  


