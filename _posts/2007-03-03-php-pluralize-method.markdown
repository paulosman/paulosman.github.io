--- 
layout: post
title: PHP Pluralize Method
wordpress_id: 17
wordpress_url: http://blog.paulosman.com/?p=17
date: 2007-03-03 19:06:41 -05:00
---
<p>I recently found myself wanting a rails-esque pluralize function like that found in the Rails Inflector class. After inspecting the Rails implementation, and playing around a bit, I was able to get a PHP version working as a Smarty variable modifier. Thank goodness for the rails version, I have no idea how I would go about listing all the various kinds of singular / plural nouns. Where would one go for a list of those kinds of things?</p>

<pre lang="php">

class MyClass 
{
    public static function conditionallyPluralize( $string, $count )
    {
        if ( intval( $count ) !== 0 )
            return MyClass::pluralize( $string );

        return $string; 
    }

    public static function pluralize( $string ) 
    {

        $plural = array(
            array( '/(quiz)$/i',               "$1zes"   ),
	    array( '/^(ox)$/i',                "$1en"    ),
	    array( '/([m|l])ouse$/i',          "$1ice"   ),
	    array( '/(matr|vert|ind)ix|ex$/i', "$1ices"  ),
	    array( '/(x|ch|ss|sh)$/i',         "$1es"    ),
	    array( '/([^aeiouy]|qu)y$/i',      "$1ies"   ),
	    array( '/([^aeiouy]|qu)ies$/i',    "$1y"     ),
    	    array( '/(hive)$/i',               "$1s"     ),
    	    array( '/(?:([^f])fe|([lr])f)$/i', "$1$2ves" ),
    	    array( '/sis$/i',                  "ses"     ),
    	    array( '/([ti])um$/i',             "$1a"     ),
    	    array( '/(buffal|tomat)o$/i',      "$1oes"   ),
            array( '/(bu)s$/i',                "$1ses"   ),
    	    array( '/(alias|status)$/i',       "$1es"    ),
    	    array( '/(octop|vir)us$/i',        "$1i"     ),
    	    array( '/(ax|test)is$/i',          "$1es"    ),
    	    array( '/s$/i',                    "s"       ),
    	    array( '/$/',                      "s"       )
        );

        $irregular = array(
	    array( 'move',   'moves'    ),
	    array( 'sex',    'sexes'    ),
	    array( 'child',  'children' ),
	    array( 'man',    'men'      ),
	    array( 'person', 'people'   )
        );

        $uncountable = array( 
	    'sheep', 
	    'fish',
	    'series',
	    'species',
	    'money',
	    'rice',
	    'information',
	    'equipment'
        );

        // save some time in the case that singular and plural are the same
        if ( in_array( strtolower( $string ), $uncountable ) )
	    return $string;

        // check for irregular singular forms
        foreach ( $irregular as $noun )
        {
	    if ( strtolower( $string ) == $noun[0] )
	        return $noun[1];
        }

        // check for matches using regular expressions
        foreach ( $plural as $pattern )
        {
	    if ( preg_match( $pattern[0], $string ) )
	        return preg_replace( $pattern[0], $pattern[1], $string );
        }
	
        return $string;
    }

}
</pre>

<p>Works like a charm!</p>

<p><b>EDIT:</b> <a href="http://kuwamoto.org/">Sho Kuwamoto</a> took this example and ran with it, creating a really robust and complete pluralization solution for PHP (and ActionScript!). Check out his improvements <a href="http://kuwamoto.org/2007/12/17/improved-pluralizing-in-php-actionscript-and-ror/">here</a>.</p>
