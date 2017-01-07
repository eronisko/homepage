---
layout: post
title: Self-cleaning &#8217;tmp&#8217; directory in Windows
date: 2017-01-08 10:00:00
---

I've recently migrated from Ubuntu 14 to Windows 10. One feature that I really
missed on Windows is the `tmp/` directory that automatically cleans itself up
upon reboot. On Ubuntu, I used `tmp/` instead of my Downloads folder and it
worked great. Here's how I've achieved the same behaviour on my Windows system.

### The target, the job
First, created my self-cleaning directory. Set it up under `C:\Users\Ernest\Temp`.

Next, I created a script that would execute the cleaning. Under Ubuntu, the
entire command would be `rm -Rf <directory>`. It turns out that using basic
Windows shell for this task is [not so great][so], so I decided to use PowerShell.
Here's the entire script:

[so]: http://stackoverflow.com/q/1965787/1167094

{% highlight powershell %}
# clear_temp_folder.ps1
Remove-Item -Recurse -Path "C:\Users\Ernest\Temp\*"
{% endhighlight %}

Not bad!

### The execution
I've used the Task Scheduler app bundled with Windows to schedule the script.
To create the task, fire up the Task Scheduler, then go **Action** -> **Create Basic Task...**. I used the following parameters:

 * Trigger: *When I log on*
 * Action: *Start a Program*

When asked for *Program/Script*, enter

    powershell

Under *Add arguments (optional)*, enter

{% highlight powershell %}
-ExecutionPolicy ByPass -File "C:\Path\To\clear_temp_folder.ps1"
{% endhighlight %}

Finally, change the executing user to `SYSTEM`. Find your task in the **Task
Scheduler Library**, right-click and select **Properties...**.

Under Security options, click on **Change User or Group...**. After entering
`SYSTEM` and closing the dialog, your Properties window should look like this:

![Alt text](http://i.imgur.com/oRWXnLo.png "Optional title")

That should be all. You can test the script right from the Task Scheduler (use
the Actions pane on the right). If the script doesn't seem to run on login (but works when executed manually), you can [enable task history][SOHistory] to de-bug.
[SOHistory]: http://stackoverflow.com/q/11013132/1167094
