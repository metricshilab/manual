# Manual

## Deploying Shiny app to Heroku

### Recipe

1. **Go to Heroku to sign up**

	* Sign up here: <https://id.heroku.com/login>

2. **Install Heroku CLI**

	* Command line Interface: <https://devcenter.heroku.com/articles/heroku-cli>

3. **Create extra R files**

	* You basically need two more configuration files placed in where your Shiny files are:

		*init.R*: installing extra R packages

	    ``` {.R}
        my_packages = c("PACKAGE 1 NAME", "PACKAGE W NAME", ...)

        install_if_missing = function(p) {
            if (p %in% rownames(installed.packages()) == FALSE) {
                install.packages(p, dependencies = TRUE)
            }
            else {
                cat(paste("Skipping already installed package:",
                p, "\n"))
            }
        }
        invisible(sapply(my_packages, install_if_missing))
	    ```

		*run.R*: configure port

	    ``` {.R}
        library(shiny)

        port <- Sys.getenv('PORT')

        shiny::runApp(
            appDir = getwd(),
            host = '0.0.0.0',
            port = as.numeric(port)
        )  
	    ```

4. **Heroku deployment**

	* Using your command line terminal, cd into the directory, then do 3 things:

		1. Create an Heroku app
	    ```
        heroku create APPNAME --buildpack https://github.com/virtualstaticvoid/heroku-buildpack-r.git
	    ```

		2. 	Set Heroku stack to 18
	    ```
        heroku stack:set heroku-18
	    ```

		3. Push it to Heroku
	    ```
        git push heroku
	    ```

And there you have it.

### Tricks of the trade

1.  **Putting everything in the top layer**
	* For this to work, remember to keep your Shiny app in the top level of the repository.

2.  **Keeping the Shiny app awake**
	*  If I am not mistaken, Shiny app has a default timeout setting where the app will go to sleep when inactive for 60 seconds. I guess there are more elegant ways to do it but you can always force the app to do some calculations so that it's not inactive.

		``` {.R}
		autoInvalidate <- reactiveTimer(10000)

		observe({
			autoInvalidate()
			cat(".")
		})
		```

### Useful links

1.  <https://elements.heroku.com/buildpacks/btubbs/heroku-buildpack-shiny>

2.  <https://github.com/virtualstaticvoid/heroku-buildpack-r>

3.  <https://shiny.rstudio.com/reference/shiny/0.14/reactiveTimer.html>


## Future work

In the future, perhaps Zt will add a bit about experience at AWS.
