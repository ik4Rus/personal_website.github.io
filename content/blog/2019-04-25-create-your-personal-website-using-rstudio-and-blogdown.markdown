---
title: Create your personal website using RStudio and blogdown
author: Ingo Lange
date: '2019-04-25'
slug: create-your-personal-website-using-rstudio-and-blogdown
categories:
  - Website
tags:
  - GitHub
  - Hugo
  - RStudio
  - Tutorial
  - Web
  - netlify
image: images/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown.jpg
---



# Create your own website 

Website are a fantastic way to represent yourself and your work. Especially in the field of data science they are a great way to present your portfolio and a great addition to GitHub repositories. In this article we will build our personal website on ourselves using RStudio and the package blogdown, deploy it and link it to GitHub so that it updates itself everytime we push new code.

For a detailed description of this approach and primarily blogdown, read the book of the package (https://bookdown.org/yihui/blogdown/) by Yihui Xie, the leading developer.

## Prerequisites
To start we only need a current version (minimum 1.2.13) of RStudio as well as a GitHub account. Furthermore we need the packages blogdown, which we install if it is not available yet.


```r
needed_packages <- c("blogdown")
new.packages <- needed_packages[!(needed_packages %in% installed.packages()[,"Package"])]
if(length(new.packages)) install.packages(new.packages)
```

# Create the project 
Next we think about which theme we want to use. A good overview of existing free Hugo themes can be found at https://themes.gohugo.io. My advice is to take the time to test some themes and find the one that suits you. In particular you should click through the demo and reflect whether you can adapt the page to your wishes without much effort. 

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/hugo_website.png" width="1200px" />

For this article we choose the theme timer on which also this website is based. If we select the theme timer on the website and go to Download, we get to the GitHub repository on which the theme is based. We copy the repository name `themefisher/timer-hugo`. Next we create a new project under File > New Project > New Directory > Website using blogdown in R Studio. As as Hugo theme we use the copied repository name and choose without sample blog posts. 

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/new_project.png" width="400px" />

After we have built the package, we should have been created next to the project file personalwebsite.Rproj, many more files and folders. What these files mean and how we can change them will be explained later on. 

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/file_structure.png" width="400px" />


# Creating the GitHub Repository 
After we created at the project and it is open, we need to activate version control to be able to interact with GitHub. To do this, we select RStudio from the menu *Tools > Version Control > Project Setup... > Git/SVN* and select *Git* as *Version control system*. After the session has restarted, version control is activated for this project. We then commit the entire project once. To do this we switch to the terminal, which is located in the tab next to the console. Alternatively we can open it via the menu under *Tools > Terminal > New Terminal *. 

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/terminal.png" width="400px" />

Now we enter the following code: 

```
git status
git add .
git commit -m "Build website based on timer theme"
```

With the first commands we check the status and see that all files are untracked. For this reason we add all files to the staging area with `git add .` and finally commit them with the third command. 

Next we'll create a new repository after we log into GitHub. This is done using the plus symbol on the top right corner. 

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/new_repo_plus.png" width="200px" />

After we name the new repository `personalwebsite` and enter a short description, we select *create repository*: A new repository is born.

<img src="/blog/2019-04-25-create-your-website-using-rstudio-and-blogdown_files/naming_new_repo.png" width="600px" />

After the creation of the repository, GitHub already suggests the code, which we will immediately use to push the created website. Wir wechseln also wieder zu RStudio und dort ins Terminal und geben den folgenden Code ein:

```
git remote add origin https://github.com/ik4Rus/personal_website.github.io.git
git push -u origin master
```

With the first command we point to our newly created github repository, with the second we push all our local commits into this github repository. If we now update the github website with our repository, we should see all our local files online. 


# Build the website and deploy it
Der nächste Schritt ist die Website tatsächlich zu erzeugen und sich anzusehen. Dazu öffnen wir die Datein *config.toml* und ändern die base URL zu "/". Darüber hinaus fügen wir einen neue Zeile ein, um insbesondere die R spezifischen Files zu ignorieren.

```
baseURL = "/"
ignoreFiles = ["\\.Rmd$", "\\.Rmarkdown$", "_files$", "_cache$"]
```
To build an actual website from the stored information, we use the blogdown command `blogdown::build_site()`. A local version of the result can be viewed with `blogdown::serve_site()`. Alternatively we choose *Addins > BLOGDOWN > Serve Site* in RStudio. This will open a browser that displays the website locally. The information that is displayed is stored under *public*. To make the change also available on github, we push the latest modifications:

```
git add . 
git commit -m "Build site and update config.toml"
git push
```

Before we start to look at the content of the website, we want to publish it online. We go to the website https://www.netlify.com and log in with our github account. Next we can deploy a page of GitHub under *Sites* using *new sites from github*. All we have to do is select our repository personalwebsite. After a short time ( Site deploy in progress ) a green link appears on the Overview page where we can find our website. So that the website is not published using this cryptic name, we can change it in netlify. For this purpose we choose the button *Site Settings* under *Overview* and find the possibility to change the name under General and Site information. After I've changed the name to ingolange, we can always find the latest version of my website at https://ingolange.netlify.com.


# Change the landing page
After we have published our website online, we can adapt our landing page. Therefore we open the file *config.toml* and edit the page according to our needs. In particular we can insert our personal information under `[params]`. For example we can adjust the text in the header with `heading = "HI, MY NAME IS YOURNAME & I AM A"`. We could also link our GitHub page directly under the header using `btnText = "GitHub Repos"` and `btnURL = "yourgithublink"`. I would recommend you to go through everything line by line and adjust it to your preferences. After our changes we use our standard workflow again to publish our latest changes: 


```r
blogdown::build_site()
blogdown::serve_site()
```

After we have made sure that all changes are displayed as desired, we push our changes in the terminal. 

```
git add . 
git commit -m "Adjust landing page"
git push
```

Now we can check at https://ingolange.netlify.com whether the changes have been implemented as desired.

# Standart workflow to write new content 

