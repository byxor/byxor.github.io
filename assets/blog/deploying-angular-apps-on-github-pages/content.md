If you're new to web development, deploying an Angular application can be a daunting task. You might hear words like **"Apache"** and **"Nginx"** and instantly begin to panic. Don't worry, they scare me too.

Luckily we can avoid all of those scary technologies and use **GitHub Pages** to host our applications. It's free, and even gives us [HTTPS](https://blog.github.com/2018-05-01-github-pages-custom-domains-https/) (the green padlock)!

![Image of GitHub Sign Up Page](/assets/blog/deploying-angular-apps-on-github-pages/lock.png)

In this tutorial, I will take you through the deployment process **step-by-step**. And no, you **don't need a backend** to do this. Woohoo!

---

**Disclaimer:** Apache and Nginx are cool, _I just haven't learned how to use them yet._

**Fun fact:** This very website is being hosted on GitHub Pages. Isn't that neat?

## Pre-requisites

For this tutorial, I'm going to assume a few things...

1. You've **already created an Angular application**.
2. You created it with the **Angular CLI** tool.
3. You're somewhat familiar with **GitHub** and **git**.

Teaching you to create Angular applications will be out of scope for this tutorial. There's plenty of fantastic material out there. If I can do it, _you can do it._

## 1. Create a GitHub Account

If you already have a GitHub account, you can skip this step.

* Go to <a href="https://github.com/join" target="_blank">GitHub's Sign Up Page</a>.
* Pick your username carefully (read below).
* Enter your details & sign up.
* Remember to pick the free plan.

**Your username is important!** It will decide the URL at which your website is located. For example, if I pick `john-smith-23` as a username, my angular app will be located at `https://john-smith-23.github.io`.

![Image of GitHub Sign Up Page](/assets/blog/deploying-angular-apps-on-github-pages/github-sign-up.png)

## 2. Create a Repository for your Website

Once you've verified your email address, you should land on a page like this...

* Click the `+` button in the top right.
* Click the `New repository` button.

![Image of GitHub Landing Page](/assets/blog/deploying-angular-apps-on-github-pages/create-repository.png)

**Note:** A **Repository** is a fancy word for a **"folder full of code/documents/files"**. In our case, our repository will contain our website's files (HTML/CSS/Javascript/Assets).

---

Now you should see the following page...

* Important: Enter `<username>.github.io` as the **repository name** (replace `<username>` with your real username).
* Make the repository **public** (because it's free).
* Initialise the repository with a **README** (it makes the process slightly easier).
* Create the repository.

![Image of GitHub Create Repository Page](/assets/blog/deploying-angular-apps-on-github-pages/create-repository-2.png)

![Image of GitHub Sign Up Page](/assets/blog/deploying-angular-apps-on-github-pages/empty-repository.png)

## 3. Configure Git

* Install [git](https://www.linode.com/docs/development/version-control/how-to-install-git-on-linux-mac-and-windows/) for your operating system.
* Run the following commands...

```bash
git config --global user.name "<username>"
git config --global user.email "<email>"
```

(Replace `<username>` and `<email>` with your **GitHub Details**).

## 4. Clone the Repository

* Run the following command to make a copy of the repository on your computer...

```bash
git clone http://www.github.com/<username>/<username>.github.io
```

(Replace `<username>` with your real username).

**Note:** Cloning the repository with SSH is preferable, but requires additional setup that's difficult for beginners. If you don't understand this sentence, **you can ignore it.**

## 5. Build your Application

* Run the following command from your angular project's directory...

```bash
ng build --prod
```

* The `--prod` flag tells the compiler to minify your HTML/CSS/JavaScript files. This makes them smaller and results in faster load times.
* The compiled files will be in the `dist` folder relative to your Angular project.

![Image of GitHub Create Repository Page](/assets/blog/deploying-angular-apps-on-github-pages/dist-tree.png)

## 6. Deploy your Application

Follow these steps in order.

### Clear the Repository Directory.

* Go to your `<username>.github.io` directory.
* Delete everything except the .git directory.

### Copy your Built Application to the Repository Directory.

* Go to your `dist` directory.
* Copy all the contents to the `<username>.github.io` directory.

### Tweak the files for GitHub Pages.

* Go to your `<username>.github.io` directory.
* Make a copy of `index.html` and call it `404.html`.

**Note:** The `404.html` file is important if your Angular App does any kind of URL Routing. Without `404.html`, people visiting your webpage will get a bunch of 404 pages from GitHub.

![Image of GitHub Create Repository Page](/assets/blog/deploying-angular-apps-on-github-pages/404.png)

This is a workaround for an issue that's currently being discussed [here](https://github.com/angular/angular-cli/issues/995).

### Upload the files to GitHub

* Go to your `<username>.github.io` directory.
* Run the following commands...

```bash
git add .
git commit -m "Deploy website"
git push
```

The files should now be visible on the GitHub website.

![Image of GitHub Create Repository Page](/assets/blog/deploying-angular-apps-on-github-pages/repository-files.png)

## 7. Done! (Mostly)

Hooray! You made it.

If everything was done correctly _(and if my instructions were clear enough)_, you should now have a website available at `https://<username>.github.io`. This may take up to 10 minutes, but definitely no longer.

![Image of GitHub Create Repository Page](/assets/blog/deploying-angular-apps-on-github-pages/website.png)

You can find the example application from this tutorial [online here](https://john-smith-23.github.io).

## 8. Optional: Use a Custom Domain Name

Do you own a domain (e.g. `john-smith.com`) that you'd like to link to your website?

* Create a file in the repository called `CNAME` that contains the domain name (`john-smith.com`).
* Now you'll have to **configure DNS** settings with the company you bought your domain name from. **Instructions vary** depending on who you bought the domain from, so I recommend you **search online** for further help.

## 9. Optional: Automate the Deployment Process

Let's face it. It quickly **becomes tedious** to repeat these steps every time you want to deploy your application.

I wrote a bash script to automate the process for me.

```bash
#!/bin/bash

if [ "$#" -ne 3 ]; then
    echo "Expecting parameters: <source_directory> <deployment_directory> <domain>"
    exit 1
fi
        
function shout() {
    local banner=$(perl -E 'say "#" x 80')
    echo
    echo $banner
    echo $1
    echo $banner
}
                            
sourceDirectory=$1
deploymentDirectory=$2
domain=$3

shout "Building and minifying website..."
(cd $sourceDirectory ; ng build --prod)

shout "Clearing deployment directory... ($deploymentDirectory)"
rm -r $deploymentDirectory/*

shout "Copying files to deployment directory... ($deploymentDirectory)"
cp -a dist/. $deploymentDirectory
                            
shout "Adjusting content to work correctly on github pages..."
cp $deploymentDirectory/index.html $deploymentDirectory/404.html

shout "Applying CNAME for domain... ($domain)"
echo $domain > $deploymentDirectory/CNAME

shout "Committing changes to version control..."
(cd $deploymentDirectory ; git add . ; git commit -am "Deploy latest version" ; git push)
```

Every time I want to deploy, I run the following command:

```bash
./deploy.sh <angularProjectDirectory> <repositoryDirectory> <domainName>
```

Deploying my website has **never been easier**. Thanks GitHub!

## Conclusion

We've **successfully deployed** an existing Angular application to the internet **for free**! It has **HTTPS** which means all traffic is **encrypted**.

We can easily update our application by **re-deploying** it to the repository we cloned. Enjoy!

## Feedback?

Having trouble? Send a tweet to [@brandnewbyxor](https://twitter.com/brandnewbyxor) and I'll try and help.

You can also tweet me if I've made a mistake so I can correct it.
