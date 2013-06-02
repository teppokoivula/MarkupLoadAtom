ProcessWire Atom Loader
=======================

Given an Atom feed URL, this module will pull it, and let you foreach() it 
or render it. This module will also cache feeds that you retrieve with it.

This module was forked from original Markup Load RSS Feed module (ProcessWire
RSS Loader) to provide similar functionality for Atom feeds. In most regards,
including method names and options, it's identical to it's RSS counterpart.

For ProcessWire 2.1+

Original code (the good parts) copyright 2011 by Ryan Cramer, conversion from
RSS to Atom copyright 2013 Teppo Koivula.


INSTALLATION
============

The MarkupLoadAtom module installs in the same way as all PW modules:

1. Copy the MarkupLoadAtom.module file to your /site/modules/ directory. 
2. Login to ProcessWire admin, click 'Modules' and 'Check for New Modules'. 
3. Click 'Install' next to the Markup Load Atom module. 


USAGE
=====

The MarkupLoadAtom module is used from your template files. 
Usage is described with these examples: 


Example #1: Cycling through a feed
----------------------------------

    $atom = $modules->get("MarkupLoadAtom"); 
    $atom->load("https://github.com/ryancramerdesign/ProcessWire/commits/master.atom");

    foreach($atom as $item) { 
        echo "<p>";
        echo "<a href='{$item->url}'>{$item->title}</a> ";
        echo $item->date . "<br /> ";
        echo $item->body; 
        echo "</p>";
    }


Example #2: Using the built-in rendering
----------------------------------------

    $atom = $modules->get("MarkupLoadAtom");
    echo $atom->render("https://github.com/ryancramerdesign/ProcessWire/commits/master.atom");


Example #3: Specifying options and using channel titles
-------------------------------------------------------

    $atom = $modules->get("MarkupLoadAtom");

    $atom->limit = 5;
    $atom->cache = 0; 
    $atom->maxLength = 255; 
    $atom->dateFormat = 'm/d/Y H:i:s';

    $atom->load("https://github.com/ryancramerdesign/ProcessWire/commits/master.atom");

    echo "<h2>{$atom->title}</h2>";
    echo "<p>{$atom->subtitle}</p>";
    echo "<ul>"; 

    foreach($atom as $item) {
         echo "<li>" . $item->title . "</li>";
    }

    echo "</ul>";


OPTIONS
=======

Options MUST be set before calling load() or render().

    // specify that you want to load up to 3 items (default = 10)
    $atom->limit = 3;             

    // set the feed to cache for an hour (default = 120 seconds)
    // if you want to disable the cache, set it to 0.
    $atom->cache = 3600;          

    // set the max length of any field, i.e. description (default = 2048)
    // field values longer than this will be truncated
    $atom->maxLength = 255;

    // tell it to strip out any HTML tags (default = true)
    $atom->stripTags = true;

    // tell it to encode any entities in the feed (default = true); 
    $atom->encodeEntities = true;

    // set the date format used for output (use PHP date string)
    $atom->dateFormat = "Y-m-d g:i a";

See the $options array in the class for more options.

You can also customize all output produced by the render() method,
though it is probably easier just to foreach() the $atom yourself. But
see the module class file and $options array near the top to see how
to change the markup that render() produces. 


MORE DETAILS
============

This module loads the given Atom feed and all data from it. It then populates 
that data into a WireArray of Page-like objects. All of the fields in the Atom 
feed are accessible, so you use whatever the feed provides.

The most common and expected field names in the Atom entry array are:

    $atom->id
    $atom->title
    $atom->updated
    $atom->published           (or $atom->date)
    $atom->subtitle            (or $atom->body)
    $atom->link                (or $atom->url)
    $atom->created             (unix timestamp of published)

The most common and expected field names for each Atom item are:

    $item->id
    $item->title
    $item->updated
    $item->published          (or $item->date)
    $item->content            (or $item->body)
    $item->link               (or $item->url)
    $item->created            (unix timestamp of published)

For convenience and consistency, ProcessWire translates some common Atom 
fields to the PW-equivalent naming style. You can choose to use either the 
ProcessWire-style name or the traditional Atom name, as shown above.


HANDLING ERRORS
===============

If an error occurred when loading the feed, the $atom object will
have 0 items in it:

    $atom->load("...");
    if(!count($atom)) { error }

In addition, the $atom->error property always contains a detailed
description of what error occurred:

    if($atom->error) { echo "<p>{$atom->error}</p>"; }

I recommend only checking for or reporting errors when you are
developing and testing. On production sites you should skip
error checking/testing, as blank output is a clear indication
of an error. This module will not throw runtime exceptions so
if an error occurs, it's not going to halt the site.


SUPPORT
=======

Visit the ProcessWire forum at http://processwire.com/talk/


Copyright 2013 by Teppo Koivula
Copyright 2011 by Ryan Cramer


