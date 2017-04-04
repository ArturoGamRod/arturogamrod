---
layout: single
title: 'Failed to load resource: the server responded with a status of 404 (Not Found)'
categories: [Angular]
tags: [Angular2+,Error,File Not Found,Relative Path,Angular,typescript]
---

This error could be caused because of how relative paths work. When we set a relative path for a property like <b>templateUrl</b> or <b>styleUrls</b> in a typescript file using "./" or "../", the resulting path is based on the currently in use path/url by the user on the web browser, in this case the path Index.html resides in: the root level e.g, www.mywebsite.com/, not the path that <b>component.ts</b> resides in e.g, www.mywebsite.com/App/. Take a look at the next example:

<b>Directory structure</b>:

```
SRC
│   Index.html
└───App
│    │   my-component.ts
│    │   my-componenet.html
│    └───styles
│         │  my-component.css
│       
│___ globalStyles 
          │  style.css

```

 <b><<Root>>/App/my-component.ts:</b>
{% highlight typescript %}

@Component({
  selector: 'my-component',
  templateUrl: './my-component.html',
  styleUrls: ['../globalstyles/style.css']
})

{% endhighlight %}

This example wont work since the relative paths are formed taking into account that these are relative to "my-component.ts" which is not true in this case. If we want to make the paths relative to my-component.ts we have to set the <b>moduleId</b> property in the module declaration.


{% highlight typescript %}

@Component({
  selector: 'my-component',
  moduleId: module.id,
  templateUrl: './my-component.html',
  styleUrls: ['../globalstyles/style.css']
})

{% endhighlight %}

For this to work we just have to make sure that <b>commonjs</b> module is being used in <b>tsconfig.json<b/> file:

{% highlight json %}

{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "lib": [ "es2015", "dom" ],
    "noImplicitAny": true,
    "suppressImplicitAnyIndexErrors": true
  }
}

{% endhighlight %}
