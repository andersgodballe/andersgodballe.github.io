---
layout: post
title: Howto use Git with Perforce
author: Anders Godballe
tags: [Git, Perforce]
categories: [Programming]
---

This is a little guide on how I got Git to work on top of Perforce

### Required Software:
* Perforce
* Python 2.7.x (in my case it was 2.7.6)
* Git (in my case it was 1.8.5.3)

### Setup:
If the software is not yet installed please install first.

Download the Git Perforce Bridge from here: https://github.com/git/git/blob/master/git-p4.py

Navigate to the directory where you installed Git and copy the downloaded file "git-p4.py" to the "bin" directory

Open up the Git Bash Console which was installed together with Git and verify that the 
Git Perforce Bridge is enabled in your Git installation by typing

{% highlight powershell %}
git p4.py 
{% endhighlight %}

if you get the following, the bridge is correctly installed:

{% highlight powershell %}
usage: C:\Program Files (x86)\Git\bin\git-p4.py <command> [options]

valid commands: clone, rollback, debug, commit, rebase, branches, sync, submit

Try C:\Program Files (x86)\Git\bin\git-p4.py <command> --help for command specific help.
{% endhighlight %}

Then we need to skip the Perforce Submit Edit dialog and instead let the Git commit message be used here.
We do that by typing the following in a Git Bash:   

{% highlight powershell %}
git config git-p4.skipSubmitEdit true
{% endhighlight %}

To make this configuration permanent create a ".gitconfig" file in your home directory with the following content

{% highlight powershell %}
[user]
    name = {FULLNAME}
    email = {EMAIL}

[git-p4]
    skipsubmitedit = true
{% endhighlight %}

Create a P4 Settings file named .p4settings in c:\users\your_username and put the following in it

{% highlight powershell %}
P4PORT=repo_url:port
P4USER=username
P4CLIENT=gitworkspace
P4EDITOR=notepad
{% endhighlight %}

Open .bashrc in your home directory typocally in c:\users\your_username.
If the file does not exist create the it and put the following in it

{% highlight powershell %}
export P4CONFIG=/c/users/your_username/.p4settings
{% endhighlight %}

Last but not least define your workspace mappings in Perforce by typing the following in Git Bash

{% highlight powershell %}
p4 client
{% endhighlight %}

You can verify the settings by typing the following:

{% highlight powershell %}
p4 info
{% endhighlight %}

### General Usage:
&nbsp;
1. Login with "p4 login" and clone a Perforce project:
 
{% highlight powershell %}
git p4.py clone //depot/path/project
{% endhighlight %}

&nbsp;
2. Make some changes to the project and commit locally to Git:

{% highlight powershell %}
git add changed_file1
git add changed_file2
git commit -m "description of what was changed"
{% endhighlight %}

&nbsp;
3. In the meantime somebody in the team submitted changes to the remote Perforce repository. Merge it to your local repository:

{% highlight powershell %}
git p4.py rebase
{% endhighlight %}

&nbsp;
4. Submit your local changes back to Perforce:

{% highlight powershell %}
git p4.py submit
{% endhighlight %}

### FAQ:
Q: I have made several commits to my local Git repository, will every commit in Perforce show up as a single commit just like in my local Git repository?  
A: Yes

Q: What does "git p4.py rebase" do?  
A: The Git Perforce command "git p4.py rebase" is the equivalent to the git command "git pull"

Q: How can I see what is happening under the hood?  
A: You can add the verbose parameter to every command to see what is going on under the hood. Example: git p4.py submit -v

Q: Where can I get more information?  
A: Check out the git-p4 manual here: https://www.kernel.org/pub/software/scm/git/docs/git-p4.html