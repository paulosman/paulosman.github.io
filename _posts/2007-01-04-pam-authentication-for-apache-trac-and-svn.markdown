--- 
layout: post
title: PAM Authentication for Apache, Trac and SVN
wordpress_id: 11
wordpress_url: http://blog.paulosman.com/?p=11
date: 2007-01-04 20:45:38 -05:00
---
<p>
I&#8217;m going to describe how to get two of my favourite development tools, Trac and Subversion, to authenticate against PAM (Pluggable Authentication Module). For those who might not be familiar, <a href="http://trac.edgewall.org/" title="Trac">Trac</a> is a wiki that has integrated SCM and issue tracking. It&#8217;s written in Python and it&#8217;s incredibly useful. I&#8217;ve been using Subversion for version control for a long time and Trac is perfect for documenting a project as you go, and for keeping track of tasks and bugs. It&#8217;s access control settings allow you to specify levels of access for different users, so I often create one group for me and any other developers, and another group for my clients. The clients group can update the wiki, create tickets and get reports or look at milestones, etc. The developers group can administer all aspects of the site. Anyway, as useful as Trac and SVN are, I started to get really sick of handling authentication for them. I used to set up htpasswd files for each repository and wiki and it got to be a real pain, especially when I wanted my clients or developers to also have email, shell access, etc. So I decided to try and get mod_pam_auth working so I could use existing system accounts for Trac and Subversion access (over SSL of course). 
</p>

<p>
mod_auth_pam is an Apache module that implements Basic authentication on top of the Pluggable Authentication Module. Unfortunately, as the <a href="http://pam.sourceforge.net/mod_auth_pam/">project</a> page says, the module is no longer being maintained which is unfortunate, but it works well enough with Apache 2.0. 
</p>
<p>
Installing the module is pretty straightforward if you&#8217;re familiar installing Apache modules. I won&#8217;t go into too much detail, but as usual you&#8217;ll need to load the module in your Apache configuration:
</p>
<pre lang="apache">
LoadModule auth_pam_module modules/mod_auth_pam.so
LoadModule auth_sys_group_module modules/mod_auth_sys_group.so
</pre>
<p>
Once that&#8217;s done you can easily set up basic authentication with PAM. Because Basic authentication involves sending a username + password combination in plain text, this setup should <b>not</b> be used without SSL. Within my VirtualHost configuration, I define separate location configs for each trac site and svn repository. It all looks something like this:
</p>
<pre lang="apache" escaped="true">
&lt;VirtualHost 123.321.123.321:443&gt;
    ServerName host.domain.tld
    SSLEngine On
    ...
    # Trac config
    &lt;Location /trac&gt;
       SetHandler mod_python
       PythonHandler trac.web.modpython_frontend 
       PythonOption TracEnvParentDir /var/lib/trac
       PythonOption TracUriRoot /trac
    &lt;/Location&gt;

    &lt;Location "/trac/tracsiteone/login"&gt;
       AuthPAM_Enabled On
       AuthType Basic
       AuthName "trac site # 1"
       Require user paul
    &lt;/Location&gt;

    &lt;Location "/trac/tracsitetwo/login"&gt;
       AuthPAM_Enabled On
       AuthType Basic
       AuthName "trac site # 2"
       Require group developers
    &lt;/Location&gt;

    # Subversion config
    &lt;Location /svn&gt;
       DAV svn
       SVNParentPath /var/svn
       SVNListParentPath On
       SVNAutoVersioning On
    &lt;/Location&gt;

    &lt;Location "/svn/repositoryone"&gt;
       AuthPAM_Enabled On
       AuthType Basic
       AuthName "Repo # 1"
       Require user paul
    &lt;/Location&gt;

    &lt;Location "/svn/repositorytwo"&gt;
       AuthPAM_Enabled On
       AuthType Basic
       AuthName "Repo # 2"
       Require group developers
    &lt;/Location&gt;
&lt;/VirtualHost&gt;
</pre>

<p>
So what we have now is two SVN repositories and two Trac wikis. For the first trac wiki and the first subversion repository, only the user 'paul' is given access. For the second, any valid user in the 'developers' group has access. Unfortunately there&#8217;s an issue with shadow passwords and this module and I&#8217;m not entirely happy with the work-around so I may have to edit this setup to use <a href="http://unixpapa.com/mod_authnz_external/">mod_authnz_external</a> or maybe I&#8217;ll eventually move to LDAP. Regardless, I find this works well enough for now and saves a lot of hassle maintaining separate authentication files. 
</p>
