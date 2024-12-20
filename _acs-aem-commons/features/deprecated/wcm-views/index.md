---
layout: acs-aem-commons_feature
title: WCM Views
description: Too many edit zones making your head spin?
date: 2015-05-19
redirect_from: /acs-aem-commons/features/wcm-views.html
tags: deprecated
initial-release: 1.10.0
---

## Why is this deprecated?

Since AEM 6.2, you should be developing in UI and not ClassicUI. This is a ClassicUI feature, so if you're using it, it means you haven't moved over to UI, which you should!

## Purpose

AEM pages can be very customizable with a plethora of drop-zones (column controls anyone!) and components. Each of these editable aspects results in drop-zones, edit-chrome hovers or editbars. This can result in an overwhelming experience for AEM Authors (and even a slow one depending on the browser/client machine!)

WCM Views is a feature that allows sets of Components' (WCMMode) Edit-ability to be toggled on the Page via the Sidekick. 


The WCM Views sets are customizable by developers by applying a ``@wcmViews (String[])`` property to EITHER a `cq:Component` node or directly on a content resource. WCM Views auto-aggregates the WCM Views available on the page to populate the Sidekick. WCM Views are derived from walking the Page and looking for `@wcmViews`, the WCM Views servlet allows for WCM Views to be specified by path (ex. for Inherited Parsys which may not have a resource on the page itself)

WCM Views has a sidekick option to completely display the WCMViews as well as only enable components for Editing that do not have an associated WCM Views (this is called "Default View").

## How to Use

### OSGi Configuration and Filter Enablement

Define a `sling:OsgiConfig` with the following attributes.

    /apps/mysite/config.author/com.adobe.acs.commons.wcm.views.impl.WCMViewsFilter.xml

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.ortg/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="sling:OsgiConfig"
    path-prefixes.include="[/content/mysite,/content/yoursite]"
    resource-types.include="[mysite/components/media/*]"
    />
{% endhighlight %}

* path-prefixes.include: Enabled WCM Views for paths that begin with these path prefixes. Default: [ /content ]
* resource-types.include: Resource types to apply WCM Views rules to. Leave blank for all. Default: <Blank>

Optionally define a `sling:OsgiConfig` with the following attributes.

    /apps/mysite/config.author/com.adobe.acs.commons.wcm.views.impl.WCMViewsServlet.xml

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="sling:OsgiConfig"
    wcm-views="[/content/mysite=inherited,header,footer]"
    />
{% endhighlight %}

* wcm-views: [<path>=<list of wcm views to always show>]


### Component Enablement

On the `cq:Component` nodes, add a `String[]` property `wcmViews` and set the value(s) to be the WCM View sets to toggle on and off.

Example.

    /apps/mysite/components/image@wcmViews=media
    /apps/mysite/components/video@wcmViews=media
    /apps/mysite/components/slideshow@wcmViews=media

Note, for this to work on Parsys, iParsys, Column Controls and their ilk, you must set the `wcmViews` property on these components and their sub-components. Example.

    /apps/mysite/components/parsys@wcmViews=drop-zones    
    /apps/mysite/components/parsys/colctrl@wcmViews=drop-zones    
    /apps/mysite/components/parsys/new@wcmViews=drop-zones    

### Content Resource Enablement

While less usual, content resources can be assigned WCM Views as well. Example:

    /content/mysite/en/jcr:content/common-header@wcmViews=header
        
### Enable WCM Views in Sidekick

Add the `acs-common.cq-widgets.wcm-views` client library so that it is included in the `cq.widgets` library.

> Note: v1.10.0 has a typo in the category name `acs-common.cq-widgets.wcm-views`; in v1.10.2 this will be corrected to `acs-commons.cq-widgets.wcm-views`

{% highlight xml %} 
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" 
        xmlns:cq="http://www.day.com/jcr/cq/1.0" 
        xmlns:jcr="http://www.jcp.org/jcr/1.0" 
        xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="cq:ClientLibraryFolder" 
    categories="cq.widgets" 
    embed="acs-common.cq-widgets.add-ons.wcm-views"
    />
{% endhighlight %}


![WCM Views](images/wcm-views-1.png)
![WCM Views](images/wcm-views-2.png)
![WCM Views](images/wcm-views-3.png)
![WCM Views](images/wcm-views-4.png)


