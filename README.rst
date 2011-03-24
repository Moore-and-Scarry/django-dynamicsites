dynamicsites
============

By UYSRC <http://www.uysrc.com/>

Host multiple sites from a single django project 

Expands the standard django.contrib.sites package to allow for:

 * Sites identified dynamically from the request via middleware
 * No need for multiple virtual hosts at the webserver level
 * 301 Redirects to canonical hostnames
 * Allows for a site to support multiple subdomains
 * Allows for a site to be an independent subdomain of another site
 * A site may have its own urls.py and templates
 * A single site may accept requests from multiple hostnames
 * Allows for environment hostname mappings to use non-production hostnames (for use in dev, staging, test, etc. environments)

Configuration
-------------

 1. Before you install dynamicsites, make sure you have configured at least 1 site in the admin panel, because once dynamicsites is installed, it will try to lookup a site from request.get_host(), and, if none exists, will always throw 404

 2. Add the app to INSTALLED_APPS ::

        INSTALLED_APPS = (
            ...
            'dynamicsites',
        )

 3. Add the middleware to MIDDLEWARE_CLASSES ::
    
        MIDDLEWARE_CLASSES = (
            ...
            'dynamicsites.middleware.DynamicSitesMiddleware'
        )

 4. Add the context processor to TEMPLATE_CONTEXT_PROCESSORS ::

        TEMPLATE_CONTEXT_PROCESSORS = (
            ...
            'dynamicsites.context_processors.current_site',
        )

 5. Configure dynamicsites by adding SITES_DIR, DEFAULT_HOST, and HOSTNAME_REDIRECTS to settings.py ::

        SITES_DIR = os.path.join(os.path.dirname(__file__), 'sites')
        DEFAULT_HOST = 'www.your-default-site.com'
        HOSTNAME_REDIRECTS = {
            'redirect-src-1.com':         'www.redirect-dest-1.com',
            ...
        }

 6. If your local environment (eg. test, dev, staging) uses different hostnames than production, set the ENV_HOSTNAMES map as well ::

        ENV_HOSTNAMES = {
            'my-site.dev':    'www.your-default-site.com',
            ...
        }

More Info
---------

More info can be found here:  http://blog.uysrc.com/2011/03/23/serving-multiple-sites-with-django/