# Front-End Performance Optimization

Contributing factors: 
	1. Number of HTTP Requests
	2. Size of assets

Performance Budgets act as target load times. 
Page weight is the total size of your site's assets.

Measurement is important, and will help you determine if you are meeting your goals or improving on them. 

### Chrome Dev Tools
Chrome Dev tools offer us a way to view all the requests made to our page. 

	Inspect -> Network -> Disable Cache -> Refresh

Here we see the Path, Method, Type, Status, Size, and Time. 
Initatior is the file that started the request. 
The Timeline shows a visual of all the requests, and lets us filter results. If we filter by Duration we can see which assets took the longest, and the Timeline view can show which assets are blocking others. 

At the bottom of the network tab, we will see a summary of the number of requests, amount of data transferred, and the time it took to complete. The time will vary depending on traffic and connection, but generally we want to reduce the size of all these numbers. 

IMPORTANT NOTE: These tools can only show your experience on your browser, server, and internet connection. They will not necessarily be the same for a viewer in another part of the world.

### Google PageSpeed
The Chrome Extension is deprecated. Go to: http://developers.google.com/speed/pagespeed/insights/. 

PageSpeed will point out items you can improve on. They are rated by Priority. Higher priority items will give larger performance results. 

## Optimizing Assets

1. Look for any 404 errors - Even though they aren't loading data, they are still sending requests, which slow the site down. 
1. Ensure that pages are only loading the assets that they need. Unused assets take up unnecessary bandwidth. 
1. A Sprite Sheet can be useful in reducing requests. If the site uses many icons and each one is an individual image, it may be useful to include them all in one sprite sheet (one HTTP request), and then only show the relevant icon using HTML/CSS. Note - don't combine images until they are optimized! 

### Images
The number of images, resolution, compression can all make an impact on page load. 
It's always best to design using SVG if possible. SVGs are particularly useful for icons.

When resizing imgages, it's best to resize them to 2x the size that will be used (for retina screens).
When saving for web, check 'Progressive' for JPG's - this loads the file in multiple passes, instead of one line at a time. 

### CSS
Avoid using the @import rule. We should avoid this because the @import is only discovered after the @import rule is discovered. The server then has to go back again, for another HTTP request. 

### Fonts
Try when possible to use hosted services. Servers have a max-number of concurrent requests that can be made. So if we can get assets from another server, this will be advantageous. 
Removing unused fonts and font-variations can reduce the size of requests.

### Creating Sprite Sheets
Best to only use Sprites for small UI images that are used in muliple pages or places.

Creating an SVG sprite sheet:
1. In a blank document create an SVG tag: 
	`<svg xmlns="http://www.w3.org/2000/svg" style="display: none;"></svg>`
2. Use the symbol element inside the SVG:
	`<symbol id="#" viewbox="SVG DIMENSIONS GO HERE"></symbol>`
3. Go to each SVG image that you want to include and copy it's path. Paste it inside the symbol element. Be sure to also copy over the viewbox information.
4. Embed the entire contents of this svg file in our html.
5. Now, anywhere you have used one of these images, replace it with an svg element (be sure to give the relevant class name). 
6. Use `<use xlink:href="#icon_id">` to link the element.

### Javascript
Try to load in Javascript in the bottom of a page, when possible. This will imporve 'percieved performance'. The page will still take the same amount of time to load, it will just look like it loaded faster to the user.
We can use 'async' to load in files, which allows scripts to be loaded in parallel. However, these scripts are exceuted as soon as they are loaded, so if some elements they act on have not yet loaded, they will not have the intended effects. 
Use hosted libraries if possible: http://developers.google.com/speed/libraries.

### Minification
Minifying removes all spaces, linebreaks, and comments in Javascript and CSS files. We can set up Gulp to do this for us.
