--- 
layout: post
title: Using YAML with the Zend Framework
wordpress_id: 29
wordpress_url: http://blog.paulosman.com/?p=29
date: 2008-02-18 18:23:18 -05:00
---
<p>One of the many great components provided by the <a href="http://framework.zend.com">Zend Framework</a> is <a href="http://framework.zend.com/manual/en/zend.config.html">Zend_Config</a>. In a nutshell, this component allows you to access configuration data (from a file, array, etc) through a nested object property based interface. Out of the box, the component supports working with XML and INI files via the <a href="http://framework.zend.com/manual/en/zend.config.adapters.ini.html">Zend_Config_Ini</a> and <a href="http://framework.zend.com/manual/en/zend.config.adapters.xml.html">Zend_Config_Xml</a> Config adapters.</p>

<p>If you're familiar with frameworks like <a href="http://www.rubyonrails.org/">Ruby on Rails</a> or <a href="http://www.symfony-project.org/">Symfony</a> however, you may notice that <a href="http://www.yaml.org/">YAML</a> support is missing. This is mostly because there is no one YAML parsing library for PHP. If you are willing to introduce another dependency to your application however, there is a really easy way to use <a href="http://spyc.sourceforge.net/">Spyc</a> to bridge YAML and Zend_Config. One of the nice things about the Zend_Config component is that it can take data right out of a PHP array (as opposed to an XML or INI file). This gives us quite a lot of flexibility, especially considering that Spyc returns parsed YAML data as a PHP array. Armed with this knowledge, the solution becomes really, really simple:</p>

<pre lang="php" escaped="true">
&lt;?php
require_once 'Zend/Config.php';
require_once 'spyc.php';

$configFile = "config.yml";
$config = new Zend_Config(Spyc::YAMLLoad($confFile));
?&gt;
</pre>

<p>Remarkably simple isn't it? Now, given the YAML config file:</p>

<pre lang="yaml">
 webhost: www.example.com
 database: 
     adapter: pdo_mysql
     host: db.example.com
     username: dbuser
     password: secret
     dbname: mydatabase
</pre>

<p>The above PHP code will create a config object that can then be used like this:</p>

<pre lang="php" escaped="true">
&lt;?php
print $config->webhost;        // prints 'www.example.com'
print $config->database->host; // prints 'db.example.com'
?&gt;
</pre>
