1. Always use relative directory. Don't use absolute directory, which is impossible for collaboration.

2. How to update R to the latest version without removing all installed packages?

   - Windows users: https://www.r-statistics.com/2013/03/updating-r-from-r-on-windows-using-the-installr-package/
   - Mac users: http://www.andreacirillo.com/2018/03/10/updater-package-update-r-version-with-a-function-on-mac-osx/

3. Fix random seeds for each parallel session by the package `doRNG`. Useful in Monte Carlo Simulation.

   ```
   pacman::p_load(foreach, doParallel, doRNG)
   foreach (...) %dorng% {
   	...
   }
   ```

   

