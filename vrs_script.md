## Opening -- In Unity

Hello from the middle of the Adirondack State Park! I'm here standing right on top of a giant red dot, placed directly on top of Johns Brook Lodge. If I look around, I'm able to view all the beautiful, well lit natural areas that I can see from the lodge; everything I can see from here is inside of the lodge's viewshed.

And in fact, if I walk away from the big red dot, I can start seeing other areas of the landscape which _aren't_ so brightly lit. Those are the areas outside the lodge viewshed, as calculated by a model I ran using GRASS GIS. If I wanted to, I can climb over to those areas to figure out why they're not included in the viewshed. This area here, for instance, is hidden behind the shoulder of that other mountain.

If I turn into a plane, I can view even more of the landscape at once. From up here, I can see more of a general pattern. The lodge's viewsheds are limited pretty harshly by the local topography; a few areas further north make me wonder if I need to limit the search radius of my viewshed algorithm.

This whole world exists on my laptop inside the Unity 3D video game engine. The entire thing was made using about 50 lines of R code, all the way from downloading data for this area to calculating the viewshed to creating the virtual environment.

## Slide 2 (Outline)

Today I want to talk about that -- why build these sorts of visualizations, why do it in code, and why I'm working on methods to make the whole process easier. 

## Slide 3

But, first things first, I should introduce myself! I'm Mike Mahoney, a PhD student here at SUNY-ESF, and I focus on visualizing spatial data as a way to help people understand large-scale systems. And I'm here today to talk about a paper I wrote with Colin Beier and Aidan Ackerman, called Interactive Landscape Simulations for Visual Resource Assessment, which is all about how visualizations can help build understanding. And to talk about that, I'm going to keep using this example of the visual impact of John's Brook Lodge.

## Slide 4

For instance, here's a photo of the lodge, as it actually exists. This is a pretty direct way to visualize the visual impact the lodge has -- if you're standing right where this photo was taken from, you can definitely see the lodge. It looks like this. 

If I was trying to visualize something that wasn't built yet, maybe I'd use a rendering instead of a photograph, but the basic idea is the same: this is a very concrete way to visualize the impact a structure has on the visual landscape at a given point.

This type of visualization is really useful when you only care about one or two points in the landscape, but doesn't scale beyond that. Renderings and site photos are labor-intensive to make and hard for a human to reason about in bulk; our brains can't keep that many different images in short-term memory at once.

## Slide 5

And so when we're thinking about visual impacts across a landscape, we tend to use more abstract visualizations to represent the same thing. Instead of photos from eye-level, we can take a birds-eye view and look at all of the areas that can see the lodge at once. This map, for instance, is the default output from the GRASS GIS viewshed algorithm -- the red dot is John's Brook Lodge, and the yellow areas are the viewsheds which can see and be seen by the lodge. 

## Slide 6

If we want, we can symbolize the same information in a different way -- so here for instance we've replaced our solid colors with aerial photography, so the "brighter" areas are now the viewsheds which can see and be seen by the lodge. 

What we've done here is create what are known as fluid representations, basically different ways to conceive of and visualize the same concept. This map, the map with bright colors, and the photo of the lodge are all attempting to communicate "this is the visual impact of the lodge"; but they all differ in how abstractly they represent that information, and as a result what sort of conclusions we can draw from them.

This is an essential part of how humans approach and understand the world around them. 

## Slide 7

My favorite example of this concept comes from Bret Victor, who talks about how we might represent someone swinging on a tire swing as a graph of their position over time, or as an equation that we could use to calculate exactly where they'll be at some time T.

Those graphical and symbolic equations are extremely useful for generating insight about the system. By abstracting away almost all the detail, we're able to more easily reason about the system as a whole. We can think about momentum, air resistance, friction. 

But without a more concrete representation, it's hard to turn that insight into understanding. There's not a lot of people who can look at that equation or a range on that graph and actually visualize where the swing will be, and certainly not quickly. Ideally you're able to represent the system at multiple levels of abstraction, and move back and forth between them.

But when it comes to spatial data as a whole, we tend to go for either extremely concrete visualizations -- like photographs and renderings -- or extremely abstract ones, like 2D symbolized maps, and we don't have easy ways right now to move between those levels. 

## In Unity

About a year ago I started wondering how we might make it easier for people working with real-world data to build these fluid representations, to look at their data at multiple levels without needing a field photographer or rendering team. 

And this is where video game engines come in. The screen I have up right now is the Unity 3D game engine, which is used to make some of the most popular video games in the world. But at its core, what Unity offers is a new way to create and interact with representations of entire worlds. Sure, most of the time people use those representations to fight aliens, but there's no reason we can't use the same technology to better understand our own actual world. 

In fact, Unity is a really useful tool for representing real-world data. The map I've been walking across is a 12 kilometer square surface, rendered at a 1 meter resolution. That's about 148 million pixels being rendered. And I'm able to use my keyboard and mouse to fly across this surface, to interactively investigate the model, all on my pretty old laptop. Out of the box, Unity does a great job at letting me explore this visualization.

With a little work, I can then add a character to this scene so that I can actually walk across the surface directly, rather than fly across it from above. And once I've done that, I can immediately toggle back and forth between these different ways of interacting with the simulation -- I can zoom way out and get a sense of the pattern of viewsheds across this area, or zoom all the way in and explore targeted areas of my data. 

I think this is a really powerful way to represent landscapes, and to allow people to move between different levels of detail when exploring their data. But when I started working on this a year ago, it wasn't exactly an easy thing to do.

Unity is not a GIS and is not really designed for representing real-world data. So I needed to find a way to make it a bit easier to build this sort of visualization.

## Slide 8

So on April 22nd this year, we published a new R package called terrainr. For those unfamiliar with R, we basically released a bunch of code that other people can now use to make these sorts of visualizations, much more easily than if they had to do everything by hand. terrainr provides both access to a lot of spatial data, provided by the USGS National Map program, and a set of tools to help visualize that data in R and in Unity.

## Slide 9

With our package, it now takes us exactly nine lines of code to download a digital elevation model and orthoimagery for the 12 kilometers around John's Brook Lodge.

## Slide 10

Another four lines of code lets us add the lodge as a big red dot to the orthoimagery, and transform our data into a format we can import into Unity.

## Slide 11

It takes 25 lines of code to run our viewshed algorithm in GRASS GIS, and save the outputs to our computer.

## Slide 12

And then 6 more lines to transform that viewshed raster into something we can use to dim and brighten our orthoimagery.

By my count, that's 44 lines of code from start to finish, which we can use to download spatial data, run this viewshed analysis, and then reproduce our Unity visualization for any area we want. 

I think that's huge. Not only is this visualization reproducible, but we can make the next one so much faster. All the work is already done.

So this is what our terrainr package helps people do. It makes this type of surface, quickly and reproducibly. What used to be a manual job that took ages now takes about ten minutes from downloading the data to walking across a surface.

And now I want to take a second to walk through the sorts of things that we can do with these visualizations inside of game engines, though not quite as quickly or reproducibly yet. 

## In Unity

For instance, let's go back to our Unity UI and stare directly at the red dot. There's an obvious problem with this representation: we aren't actually trying to show the viewsheds for a flat red circle, but rather a 3D building. 

Well, we can add 3D models on top of this world to make the representation better. I don't have a 3D model of John's Brook Lodge, but we did buy one of a wind turbine, so I'll use that as an example.

We can drag and drop our turbine right on to the red spot here to add it to our scene. Now, when I drop in to walk around the scene, I can actually observe the turbine from different locations on the surface. We can quickly make this representation a bit more concrete.

Now, our viewshed here wasn't made with something so tall as the turbine in mind, so a lot of the dim areas can still see at least the blades. But if I pause the game here, I'm able to tweak the turbine itself -- size, rotation, position -- and then see how that changes the setting.

I can also do a lot to make the scene more detailed. For instance, if this is actually somewhere in the middle of the Adirondacks, then it stands to reason there ought to be more trees here. Since I've got a handful of trees on my computer, I can drag and add them to the scene just like the turbine. And we could do this for a while to make a highly detailed simulation of the area.

## Slide 13

If you put a good bit more time and effort into this process, you can make absolutely stunning simulations of real-world areas. This, for instance, is a screenshot from a simulation made by Aidan Ackerman, and at a glance you might not even notice it isn't a photo. 

But of course, time and effort are expensive things. Our goal is to try and make placing objects on surfaces just as easy as building the surface itself is with terrainr -- so that writing a few lines of code once lets you place objects defined in GIS data, inside the game engine, in a clear and reproducible way.

These simulations will never replace photographs or renderings. We've got a good bit of work to do in evaluating how users perceive these sorts of visualizations before we'll know exactly what sorts of situations and projects they're appropriate for. But they might let us quickly and easily make simulations of areas that we care about, so that we can more readily identify the areas that are worth the expense of making those more concrete visualizations, and provide a new way for us to understand our world across multiple scales. 

We aren't there yet, but we're working on it.

## Slide 14

And that's my time! I want to thank everyone for coming today. I also want to thank the State University of New York for supporting this project via the ESF Pathways to Net Zero Carbon initiative.

If you'd like to learn more about terrainr, I included links to the GitHub repo and the documentation website in the slides, which are available from GitHub.

You can find me at mikemahoney218 on both GitHub and Twitter.

Thanks again, and I look forward to seeing you in the Q&A session!

