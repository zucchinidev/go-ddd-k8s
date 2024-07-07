I like the idea of the dev team give the ops team a sort of blueprint of the project,
so they can run it the way they want to.

## Project Structure

The project structure is a difficult problem to solve. I think all of us we lived the pain of a
project that is not well-structured. The Go team created an article about this problem, but I find it
very vague. So, I am going to be very opinionated here, and create the structure that I think is the best
after a few years of experience working with different projects. You may want to change the names of things,
but I think the project layout will be good for you. The project is structured in the form of layers.

There are some studies that say that a person is not able to maintain more than 7 things in their head at the same time.
I tend to reduce this number to 5. You may think that this is too few, but I think that it will help to reduce the
cognitive load of the developers, and create a better mental model of the project because when things fail it is
our mental model that is going to help us to solve the problem, no debuggers. Debuggers are good at tracing the code,
to give you the ability to develop a mental model of the project, but they are not good to solve the problem for you.
> "Debuggers are like a gun, they are good to have, but you don't want to use them."

> "Debuggers do not find bugs, they just run them in slow motion."
> Kent Beck

I want to be out of the debugger as much as possible, and I think that a good project structure can help with that.

I want to use this rule of five, when we will never have more than 5 layers within the scope of a layer. Ideally
3 will be the best number, but it is hard to achieve. I think that 5 is a good number to have a good mental model of the
project.

### The layers are:

#### The App Layer
The app layer. The responsibility of this layer is for application startup, application shutdown, it receives external
inputs, and sends external outputs (I am being very generic here). If the application layer is doing more than that,
I will argue that it is doing too much.

#### The Business or Domain layer
The business or domain layer. This layer is responsible for the business rules of the application. It is the heart of the
application. It is the most important layer of the application. It solves all the problem that our business has, and the
code that there is here is going to be reusable across multiple apps.
* core: The core has the business packages. It is split in different domains that we have in our business.
* data: The data package is responsible for the data management of the application. It is stuff related to with what we
are we doing in this project with data.
* web: reusable stuff related with our web API's. Auth, debug, etc.
#### The Foundation Layer
This is our foundation code. This is the standard library for this project. It is the code that is going to be reused.
Most of this packages will end up in their own repository. This is the code that is going to be shared across multiple
projects. This is the code that is going to be maintained by the community. These packages cannot be imported each other.
Very strong rules here. No configuration in this layer, everything has be passed in, no loggers in this layer.

#### Special mention to the vendor folder
Vendor folder is a special folder. It is not a layer. It is a place where we put the code that we are using that is not
ours. It is the code that we use from third parties. When vendoring, we can get diffs when updating the dependencies, we
can debug stuff in there easier, and we will have all the code we need for the project.

#### The Configuration Layer
Zarf (An ornamental container designed to hold a coffee cup and insulate it from the hand of the imbiber):
The configuration layer. This layer is responsible for the configuration of the application. We will build a leaf
that we will use to wrap it around the hot containers represented by the application. This is a zarf. Our protection. 

#### Our Layers and Important Considerations
* app
* business
* foundation
* vendor
* zarf

Imports always have to go down, not up! This is a very important rule. If you have a package that is importing a package
that is above it, you have a problem. It is a no-no. 