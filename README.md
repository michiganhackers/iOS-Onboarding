# iOS-Onboarding
2024 iOS Onboarding Project (WIP)

Sourced from [this YouTube Tutorial
Series](https://www.youtube.com/watch?v=Jhf3CNs8I-I&list=PLwvDm4Vfkdpha5eVTjLM0eRlJ7-yDDwBk&pp=iAQB)

All credit goes to Swiftful Thinking (a fantastic resource)!

# Setup
## Git
### Getting an SSH key:
Run these commands in your terminal (note you should change your email in the
second command)
```
ssh_key_file=~/.ssh/iOSTeam

ssh-keygen -t ed25519 -C "your_github_email" -f $ssh_key_file

eval "$(ssh-agent -s)"

ssh-add $ssh_key_file

cat $ssh_key_file.pub | pbcopy
```
- Go to [https://github.com/settings/keys](https://github.com/settings/keys) and
login.
- Click ’New SSH key’ and add your copied public key and a title (i.e. “iOS
  key”).
- Add the following lines to the file ~/.ssh/config (create it if it doesn’t
  exist):
```
Host github.com
    ForwardAgent yes
    IdentityFile ~/.ssh/iOSTeam
```
### Cloning the repository:
```
git clone git@github.com:michiganhackers/iOS-Onboarding.git
``` 
### Making your branch:
```
git checkout -b your_name
```
### Before you leave (skip this at the start):
```
git add .
git commit -m "Your commit message here"
git push
```

Ask Finn or Parker if you have any questions!!

# Xcode
- Download the latest version of Xcode from the App Store
- Open Xcode
- Create New Project
- Select iOS, select App, select Next
- Product name: Map, select Next
- Store the project in the iOS-Onboarding folder you cloned
- Select create

# What's in the basic App template?

1. `MapApp.swift`: This performs an initial set up, then creates and displays
   your initial view.
2. `ContentView.swift`: This is our initial piece of user interface.
3. `Assets.xcassets`: This is an asset catalog, which stores all the images and
   colors used in our project.
4. `Preview Content` folder containing a `Preview Assets` catalog.
5. Template code for writing tests.

The part we really care about ContentView.swift. This file contains the code
that is shown on the screen.

First, though: what makes ContentView.swift get shown on the screen?

Open `MapApp.swift`.

That code creates a new ContentView instance and places it inside a window group so
it’s visible onscreen.

Open ContentView.swift and let’s look at some actual SwiftUI code.

ContentView conforms to the `View` protocol. Everything you want to show in
SwiftUI needs to conform to `View`, and really that means only one thing: you
need to have a property called `body` that returns some sort of `View`.

The return type of body is `some View`. The `some` keyword was introduced in
Swift 5.1 and is part of a feature called opaque return types, and in this case
what it means is literally “this will return some sort of View but SwiftUI
doesn’t need to know (or care) what.”

Inside the body property there’s a vertical stack (`VStack`) of content showing an
image and some text.

That stack has a `padding()` method call below it. In SwiftUI this
actually creates a new view with padding around it, rather than changing the
existing stack. As a result, we refer to functions like `padding()` as modifiers because they create
modified content. There are also modifiers to make the image scale bigger and 
change its color for example.

Finally, below ContentView is #Preview, which marks special code to display our
view an interactive preview of our view inside Xcode. Right now this creates an
instance of ContentView, but you can customize these if you need to.

# Importing Assets

There are some assets included in the `Data` folder for use in your project.

- Right click the `Assets` folder in your Map project (Clicking the folder in the
  top left shows the "Project Navigator"), click Delete and click Move to Trash
- Drag and drop the `Assets.xcassets` folder in the `Data` folder of the cloned
  repository into your project navigatior, check "Copy items if needed" and click Finish
- Click on `Assets` in your Project Navigator in Xcode and look around inside
  the folder

# MVVM

Here is what ChatGPT has to say about MVVM: 

```
MVVM stands for Model-View-ViewModel, and it is an architectural design pattern
commonly used in Swift development (and other programming languages) to
organize and structure code in a way that separates concerns and promotes
maintainability. Here's a brief overview of each component in MVVM:

    Model: Represents the data and business logic of the application. It is
responsible for retrieving, processing, and storing data. In a Swift
application, models are often Swift classes or structs.

    View: Represents the user interface and is responsible for displaying the
data to the user. In the context of iOS development with Swift, views are
typically implemented as UIView or UIViewController instances.

    ViewModel: Acts as an intermediary between the model and the view. It
transforms the data from the model into a format that can be easily presented
by the view. The ViewModel does not have a direct reference to the view;
instead, it exposes data and commands that the view can bind to. This helps in
separating the presentation logic from the UI.

In the MVVM pattern, the flow of data is typically unidirectional: from the
model to the ViewModel, and from the ViewModel to the View. This separation of
concerns makes the codebase more modular, testable, and easier to maintain.
Additionally, it facilitates the use of data binding, where changes in the
ViewModel are automatically reflected in the associated View, and vice versa.
```

- Right click in the Project Navigator and click "New Group" to create a new
  folder
- Create 3 folders called "Views", "ViewModels", and "Models"

# Creating Models for each location

- Right click `Models`
- New File
- Swift File, Next
- Call it `Location`, create
- Inside of the new file create a struct called `Location`

## What data do we need to store in each Location?

The provided `LocationDataService` file contains an array of Location structs,
(go ahead and look inside this file).

One instantiation of a `Location` look as follows:

```
Location(
            name: "Colosseum",
            cityName: "Rome",
            coordinates: CLLocationCoordinate2D(latitude: 41.8902, longitude:
12.4922),
            description: "The Colosseum is an oval amphitheatre in the centre
of the city of Rome, Italy, just east of the Roman Forum. It is the largest
ancient amphitheatre ever built, and is still the largest standing amphitheatre
in the world today, despite its age.",
            imageNames: [
                "rome-colosseum-1",
                "rome-colosseum-2",
                "rome-colosseum-3",
            ],
            link: "https://en.wikipedia.org/wiki/Colosseum")
```

Here you can see what properties we plan to store in each Location struct.

Add each these properties to your `Location` struct like so:

```
struct Location {
    let name: String
}
```

The `let` keyword defines a constant (`var` defines a mutable/variable type) and `:String` specifies the type of the constant as a `String`.

Swift is a dynamically typed language, meaning the compiler can figure out the
types of the variables when they are given values. This means if we were to
give `name` a default value, by writing say `let name = "Michigan"`, we
wouldn't have to specify that the type is a `String`.

Finish the rest of the struct, note the type: "CLLocationCoordinate2D" is
defined in the `MapKit` library so, you should write `import MapKit` above the
struct. 

Also note `imageNames` is an array, so you should define its type in a similar
manner to the following (but not an int):

`let numbers: [Int]`

Ask Finn if you have any questions.

## Add DataServices
- Make a new folder called `DataServices`
- Drag and drop the `LocationsDataServices` file from the "Data" folder into
  your new `DataServices` folder, again making sure to copy if needed.

Note: since we created the `Location` struct, this file shouldn't cause any
compiler errors.

# Our first View and ViewModel
- Add a new file to Views called `LocationsView`, this time selecting "SwiftUI
View" instead of "Swift File"
- Add a new (regular swift file) file to ViewModels called `LocationsViewModel`

Make a class called LocationsViewModel and have it "conform" to the
`ObservableObject` "protocol" like so:

```
class LocationsViewModel: ObservableObject {
    
}
```
- What is the difference between `class` and `struct` in swift? Do a quick google search.

Notice how the above syntax looks similar to the excplicit defining of data types we used
earlier. 

What this does is allows the changes to the properties of our class to
trigger updates to the user interface.

Protocols can be thought of as "Types" of classes. For a class to conform to
a protocol, it means the class is guaranteed to define certain functions and
properties specified by the function.

Now lets add a variable to this viewmodel to store the array of locations
defined in `LocationsDataService` like so:
```
@Published var locations: [Location]
```

The `@Pulbished` wrapper makes it so the `locations` variable "announces" to
the rest of the program when it changes, allowing it to be updated on the
screen.

We can also add an `init()` constructor to the class to intilize the locations
variable with the array in `LocationsDataService` like so:
```
init() {
    let locations = LocationsDataService.locations
    self.locations = locations
}
```

## Using our ViewModel

Inside of our `LocationsView` file, lets try to display the locations in
a list.

Lets create an instance of our `LocationsViewModel` at the top of our struct
like so:

```
@StateObject private var vm = LocationsViewModel()
```

-- Temporary end of Tutorial ---
Go to the video playlist linked at the top, video #2 to continue
