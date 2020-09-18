# Using imgix to Test Client Hints and Deploy in Vercel

I created a simple [demo site](https://ch-testing.vercel.app/) to show this working.  It's important to note that I tried to make this site as simple as possible with minimal css to ensure any changes to the images was purely happening because of the Client Hints.  Please note that if you open a specific image in a separate tab by itself, it will not appear the same size as used on the website. It only sizes according to Client Hints when on the actual site.

I've included 9 example images. Each image code is like this:

```
<img src="https://imgix.tomdale.website/artsy/5.jpg?ch=width,dpr&w=400&ar=4:3&fit=crop&crop=faces,edges&auto=format,compress" 
sizes="(min-width: 768px) 30vw, 100vw" />
```

While in a supporting browser, like Chrome, the images will we 30% the width of the browser width when the browser is 768 pixels or larger. Once it goes smaller than that, each image will be sized at 100% the width of the browser. This is truly a dynamic size, every time you resize this browser and do a hard refresh you will notice the weight of the images change in the network tab.

In an ideal world, you should also be using srcset in combination with Client Hints.  You do want to serve a good size image for the other 25% of users that aren't supported for Client Hints.  In this demo I did not do that because I wanted to focus on showing the size of these images.  You will also notice the cropping points of these images change a bit, that's because I am using some imgix API to automatically crop to a face first, then a prominent object in the photo.

# Deploying to Vercel

I enjoy using both Vercel & Netlify to deploy my sites.  They both have a lot of benefits.  In this example I did use Vercel.  The important item needed to make Client Hints to work is setting a feature policy.  With Vercel, you can set that policy by adding a vercel.json file. For my demo I put this in the vercel.json file:

```
{
  "routes": [
  	{
      "src": "/*",
  		"headers": {
  		  "Accept-CH": "DPR, Width, Viewport-Width",
  		  "Feature-Policy": "ch-dpr https://imgix.tomdale.website 'self'; ch-width https://imgix.tomdale.website 'self'; ch-viewport-width https://imgix.tomdale.website 'self'"
  		}
  	}
  ]
}
```

You should obviously change that url to the url you are using for your images to make this work.  But, in order to serve Client Hints safely, this isn't that big of an effort to add.

# Article on Dev.to
Find the full article on [Dev.to](https://dev.to/daletom/client-hints-is-back-deploy-with-imgix-vercel-4abb)
