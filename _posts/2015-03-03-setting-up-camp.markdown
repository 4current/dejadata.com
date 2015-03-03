---
layout: post
title:  "Setting up camp"
date:   2015-03-03 03:28:47
categories: blog standup
---

Jekyll - it looks so easy but there are many digressions. I have settled
on the most straightforward path for now.Originally I wanted to start off
with bootstrap, however, the combination of jekyll and bootstrap started
to get pretty sticky. So I went back to kicking off with just jekyll. I
will however throw in Mathjax because it is so pretty.

So here is the history of how to set up. 

Create a user called jekyll
{% highlight bash %}
# useradd -m jekyll
{% endhighlight %}

Now we want the service to run as jekyll, but jekyll cannot access port
80. So we need to map that tho a port above 1024. jekyll defaults to
4000 so let's map 80 to 4000

### Upstart script
Below is a statup script
{% highlight bash %}
start on runlevel [2345]
stop on runlevel [!2345]

setuid jekyll
setgid jekyll
script
        chdir /home/jekyll
        exec ./start_jekyll
end script

{% endhighlight %}

{% highlight bash %}
#!/bin/bash
#set -x

server=dejadata.com
logfile=~/server.log

if [ $USER != 'jekyll' ]; then
    echo "you must be user 'jekyll' to execute this script"
    exit
else
    trap "pkill -9 jekyll; echo exiting on signal >>$logfile" EXIT
    cd ~/$server
    jekyll serve -BV 2>&1 >> ~/server.log
    while :
    do
        sleep 100d
    done
fi
~                                                                                                                                           

{% endhighlight %}
