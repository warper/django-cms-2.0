Configuration
=============

Out of the box django-cms comes not with a lot features. But with some settings you can extend it to a enterprise ready solution.
All settings described here can be found in cms/settings.py or in the plugins folder if they have a settings.py

All thoose settings should be placed in your project settings.py

CMS\_TEMPLATES 
--------------

A list of the templates you can select for a page.

Example:

	CMS_TEMPLATES = (
		('base.html', gettext('default')),
		('2col.html', gettext('2 Column')),
		('3col.html', gettext('3 Column')),
		('extra.html', gettext('Some extra fancy template')),
		('INHERIT', gettext('Inherit template')),
	)

Adding the magic INHERIT is optional and allows pages to
inherit the template from their nearest ancestor. Adding new
pages defaults to this iff the new page is not a root page.  

CMS\_PLACEHOLDER\_CONF
----------------------

Is used to configure the placeholders.

Example:

	CMS_PLACEHOLDER_CONF = {
		'content': {
			'plugins': ('TextPlugin', 'PicturePlugin'),
			'extra_context': {"theme":"wide"},
			'name':gettext("Content")
    	},
    	'right-column': {
        	"plugins": ('TeaserPlugin', 'LinkPlugin'),
        	"extra_context": {"theme":"small"},
			'name':gettext("Right Column")
    	},
	}
	
**plugins**

A list of plugins that can be added to this placeholder. If not supplied all plugins can be selected.

**extra\_context**

Extra context that plugins in this placeholder receive.

**name**

The name displayed in admin. With the gettext stub it can be internationalized.


CMS\_PERMISSION
---------------

Example:

	CMS_PERMISSION = True
	
If this is enabled you get 3 new models in Admin:

- Pages global permissions
- User groups - page
- Users - page

In the edit-view of the pages you can now assign users to pages and grant them permissions.
In the global permissions you can set the permissions for users globaly.

If a user has the right to create new users he can now do so in the "Users - page". But he will only see the users he created.
The users he created also can only have the rights he already has. So if he has only the right to edit a certain page all users he created also only can edit this page. Naturally he can even more limit the rights of the users he creates.


CMS\_MODERATOR
--------------

Example:

	CMS_MODERATOR = True
	
If set to true gives you a new column "moderation" in the tree view.
You can select to moderate pages or hole trees. If a page is under moderation you will recieve an email if somebody changes a page and you will be asked to approve the changes. Only after you approved the changes they will be updated on the live site. If you change a page you moderate yourself you will need to approve it anyway. This allows you change a lot of pages for a new version of the site and can go live with all the changes on the same time.



CMS\_SHOW\_START\_DATE & CMS\_SHOW\_END\_DATE
----------------------------------------------

Example:

	CMS_SHOW_END_DATE = True
	CMS_SHOW_START_DATE = True
	
This adds 2 new date-time fields in the advanced-settings tab of the page.
With this option you can limit the time a page is published.

CMS\_URL\_OVERWRITE
-------------------

Example:

	CMS_URL_OVERWRITE = True
	
This adds a new field "url overwrite" in your in the advanced-settings tab of the page.
With this field you can overwrite the whole relative url of the page.


CMS\_MENU\_TITLE\_OVERWRITE
---------------------------

Example:

	CMS_MENU_TITLE_OVERWRITE = True

This adds a new field "menu title" besides the title field.
With this field you can overwrite the title that is displayed in the menu.

To access the menu title in the template use:

	{{ page.get_menu_title }}

CMS\_REDIRECTS
--------------

Example:

	CMS_REDIRECTS = True
	
This adds a new field "redirect" to the advanced-settings tab of the page
You can enter a url and if someone visits this page he gets redirected to this url.

Note: Don't use this too much. There is django.contrib.redirect for this purpose

CMS\_SEO\_FIELDS
----------------

Example:

	CMS_SEO_FIELDS = True

This adds a new Fieldset "SEO Fields" in the page. You can set there the Page Title, Meta Keywords and the Meta Description

To access the fields in the template use:
	
	{% load cms_tags %}
	<head>
		<title>{% page_attribute page_title %}</title>
		<meta name="description" content="{% page_attribute meta_description %}"/>
		<meta name="keywords" content="{% page_attribute meta_keywords %}"/>
		...
		...
	</head>


CMS\_FLAT\_URLS
---------------

Example:

	CMS_FLAT_URLS = True
	
If this is enabled the slugs are not nested in the urls.

So a page with slug "world" will have an url "/world" even it is a child of the page "hello"
If disabled the page would have an url: "/hello/world/"


CMS\_NAVIGATION\_EXTENDERS
--------------------------

Example:

	CMS_NAVIGATION_EXTENDERS = (
		('shop.navigation.get_nodes', 'Shop Categories'),
		('gallery.navigation.get_nodes', 'Galleries'),
	)


Navigation extenders allow you to extend the menu. 
In Brief: A tuble with a python path to a function that returns a list of navigation nodes	
For detailed explanation have a look in navigation.md in the section "Extending the menu".

This gives you a new field in the advanced-settings tab of the page. You can there say that one navigation extender is attached to this page.

CMS\_APPLICATIONS\_URLS
-----------------------


Example:

	CMS_APPLICATIONS_URLS = (
    	('someapp.urls', 'Some application'),
    	('sampleapp.urls_en', 'Sample application English URLs'),
    	('sampleapp.urls_de', 'Sample application German URLs'),
	)

You can let cms handle the urls of other 3th party apps and attach them to pages.
For example you have a page named blog and a blog application. You can attach the blog urls.py to the blog page. After this the every url after blog page will be handled by the blog application. The nice thing about this is the cms can be used in the templates of the blog application. So you can use placeholders in the templates and they will be filled up with plugins that are defined in the blog page. Also the blog menu item will be selected if you are on a page that is handled by the blog application.

At the moment if you change this settings you should restart the server as we some cache invalidation bug. 

CMS\_UNIQUE\_SLUGS
------------------

Example:

	CMS_UNIQUE_SLUGS = True
	
Defines if the slugs should be unique over all sites and languages.
This setting is changed automatically according to other settings.
Do not set it in you settings.py if you don't know what you are doing.

CMS\_SOFTROOT 
-------------

Example:

	CMS_SOFTROOT = True
	
This adds a new field "softroot" to you advanced-settings tab in the page.
If a page is marked as softroot the menu will only display the items till the softroot. If you have a huge site you can partition the menu with this.

CMS\_HIDE\_UNTRANSLATED
-----------------------

Example:

	CMS_HIDE_UNTRANSLATED = False
	
By default django-cms hides the menu items that are not translated yet in the current language. With this setting set to False they will show up anyway.


CMS\_LANGUAGE\_FALLBACK
-----------------------

Example:

	CMS_LANGUAGE_FALLBACK = True
	
This will redirect the browser to an same page in an other language uf the page is not available in the current language.

CMS\_LANGUAGES
--------------

Which language should be used by the cms?

Example:

	CMS_LANGUAGES = (
	    ('fr', gettext('French')),
	    ('de', gettext('German')),
	    ('en', gettext('English')),
	)
	
Normally the cms takes the LANGUAGES from the settings.py. With this you can overwrite this. Be sure that you don't have more language in here than in the LANGUAGES setting.


CMS\_DEFAULT\_LANGUAGE
----------------------

Example:

	CMS_DEFAULT_LANGUAGE = "de"
	
The default language is used if no other language could be found in the browser. Normally will the take the first of the LANGUAGES setting.


CMS\_CONTENT\_CACHE\_DURATION
-----------------------------

Example:

	CMS_CONTENT_CACHE_DURATION = 60

Defines how long page content should be cached, including navigation and admin menu.
Default is 60

CMS\_MEDIA\_PATH
----------------

Example:
	
	CMS_MEDIA_PATH = "cms/"
	
The path from MEDIA\_ROOT to the media files located in cms/media/
default: "cms/"

CMS\_MEDIA\_ROOT
----------------

Example:

	CMS_MEDIA_ROOT = "settings.MEDIA_ROOT + "/cms/"

The path to the media root of the cms media files. 
Default: settings.MEDIA_ROOT + CMS_MEDIA_PATH

CMS\_MEDIA\_URL
---------------

Example:

	CMS_MEDIA_URL = "/media/cms/"
	
The location of the media files that are located in cms/media/cms/
default: MEDIA\_URL + CMS\_MEDIA\_PATH

CMS\_PAGE\_MEDIA\_PATH
----------------------

By default the cms creates a folder in called 'cms\_page\_media' in your static files folder where all uploaded media files are stored.
The media files are stored in subfolders numbered with the id of the page.

Example:

	CMS_PAGE_MEDIA_PATH = 'cms_page_media/'


