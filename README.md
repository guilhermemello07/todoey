# todoey

The purpose of this readme file is to give an in-depth explanation of the project line by line, not to explain it for another programmer, but for myself. I can say that I found it essential to explain like this, in order to truly make the knowledge my own, as the majority of the projects nowadays are made in a “code along” format, it's essential for me to be able not only to use the techniques but also to explain how and why I used them.

So, if you found this approach boring, please consider that this was created for my own study.

## ⚠️ Project Overview

![94ABDC54-AC37-4A91-AB90-7D282E88F331_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/6e453dbd-4925-4916-8d36-d159b8a0110a)
The project was made following the guidelines of the MVC (Model-View-Controller) design pattern.

### View
The view contains the main view of the app, which was made using UIKit, and has all the UITableView elements in the main file.

### Model
The Model part of the project is built upon the Core Data framework, which Xcode has already a built-in interface that we can work with in order to get a lot of functionality already done, so the only file that I have there is the DataModel file, which is a Core Data file with two entities, the Item entity, and the Category entity, more on those later.

### Controller
The Controller folder is the one, as expected, with more logic happening.
Here I have three files, the SwipeCellTableViewController, which is a subclass of UITableViewController, and adopts the protocol SwipeTableViewCellDelegate in order to conform to the SwipeCellKit library that I used in this project; the CategoryViewController, which is a subclass of SwipeCellTableViewController, and the TodoListViewController, which is a subclass of SwipeCellTableViewController, and adopts the UISearchBarDelegate protocol.

## ⚠️ Purpose of the App.

This app is a to-do list app, in which the user can create categories and can create items to do inside each category, so, for example, if the user intends to use the app to remember the things that he needs to buy at the supermarket and also a task that he needs to complete in the work, he can create a category named “Shopping list” and another named “Work”, for example.

Inside each category, the user can create items to do, for example, inside the Shopping list category the user can create items that he intends to buy, apples, bananas, rice, water, soda, pasta, etc… and when the user has already completed any of the to-do items, he can tap into the item and mark it with a ✅ mark, or he can delete completely the item.

The app also provides the capability to delete Categories, but if a category that has some items inside is deleted, then the items are considered to be deleted too.

So, the app does basically this, and here, the technical feature of the app is the fact that all the user data is persistent, or better said when the user closes the app, updates the app, changes iPhone, etc., the data generated by the user remains accessible and visible in the app. 

To persist data in this project, I’ve decided to use Core Data, and although I could have used Realm, I think that, as this project’s intent is to showcase some technical abilities, and Core Data has a broadly professional use than Realm, I decided to use it.

## ⚠️ Technologies Used

To build this App, I have used the following technologies:

- Swift
- Xcode
- UIKit
- Core Data
- SwipeCellKit
- Swift Package Manager

## ⚠️App Delegate
![DF5FF16D-0371-4F43-876E-D079A9F3541B_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/2dbecb75-482c-481c-8869-e66ff71e6c16)
Inside the AppDelegate file, I have imported CoreDate to be able to use it here, and according to the documentation of CoreData, we need to configure some simple steps inside this file.

In the applicationWillTerminate, which is the function that gets called by the system before the app terminates, we need to call self.saveContext(), and this method just grabs the context we have created (more on that later) and saves it. (This makes the data persist even after the app gets terminated, I.e. when the user closes the app).

In the CoreData Stack part of the code, I have created the persistentContainer, which is a lazy variable, or better said, a variable that only allocates space in memory when it gets initialized. The persistentContainer is of type NSPersistentContainer and is when we can access the viewContext, which is the context we need to save when we want to persist data locally.

In the CoreData Saving Support part of the code, I just created the saveContext() function, and here I just grabbed the context from our early created persistentContainer.viewContext and looked to see if the context has changes on its data, if it has, then we save the context.

## ⚠️View
![6A28BA27-29F3-4666-B62E-ABF2AD90FD0D_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/b98e731e-5fc7-40e4-be05-a10b47068f44)

The view for this project is really simple, as I used the built-in functionality that the UIKit gives for us when we use a TableViewController, and here, I have created two TableViews, one for the CategoryViewController, and one for the TodoListViewController. 

Both TableViews are embedded in a NavigationController.

## ⚠️Model
![30BAA64A-3122-4EE5-9EE8-B8BEF04743AF_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/33bf922e-0092-47a6-a036-f466a28bafbd)

### The Category entity

The Category dataModel is meant to store categories created inside the app, and they are structured with just one attribute and one relationship, the attribute is the name of the category, how the user chooses to call that category, and the relationship is how the Category interacts with the Item entity, and here I defined the relationship from category to the item as items, which is a toMany relationship since each category can have many items associated with them.

![3889D754-2438-40BC-AA32-71B81C542FF1_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/00d3f2a0-1ebd-484c-9e29-0485c00a1822)

### The Item entity 

This entity was created to store items the user creates inside each category.

Each item has a title and a done property, The title property is how the user chooses to call the item, and the done property defines if that specific item is done or not, and for default, every item is created with the done property set as false, since it makes no sense to create an item as it’s already done.

The relationship from Item to Category is defined as parentCategory and is a toOne relationship since items can only have one parent category.

## ⚠️Controller

### SwipeCellTableViewController
![A4C2F7F0-F850-465D-8A13-DDBBF753E4F3_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/6ecaed32-57bd-4606-90c3-25b1b89ced9f)
The SwipeCellTableViewController was created as a subclass of UITableViewController and its purpose is to give modularity to the code, as it’s implemented without any knowledge of the CategoryViewController as well as the TodoListViewController, which subclasses it.

Inside this class, I’ve defined the rowHeight of the cells to a fixed number, which I found better as 70.0, and inside the Table view data source part I’ve just created and returned the dequeued reusable cell, for the specific indexPath, as this function will be called inside the subclass in order to give more flexibility in the customization, but making this call to cellForRowAt IndexPath here makes all the cells swappable! Which is the main goal of this class.

The swipe table view cell delegate methods part of the code I just read the documentation related to the package, and implemented the code obviously, observing the context and making the adjustments needed.

Worth mentioning here that I just defined a function called updateModel(at indexPath) here, and called it inside the editActionsForRowAt..() function, in order to allow the subclasses of this class to edit their dataModel without giving this class any information about it.

### CategoryViewController
![A6465946-9E11-4CE0-9334-AC258AA8003A_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/385b5da8-8a08-479d-a2cb-446ed197f5e8)
Part 1
First of all, in order to make its cells swappable, the CategoryViewController is a subclass of my previously created SwipeCellTableViewController class.

In the CategoryViewController I first created both the categoryArray, which is an array of Category objects created by the CoreData dataModel, and initialized it as an empty array, and I also created a variable named context in order to access the persistentContainer.viewContext inside the AppDelegate file, and since the AppDelegate in a shared instance, I need to access it directly without creating an instance of it in every class that I need to access its properties.

Inside the Table view data source methods I just defined the two methods that I need for this app, the numberOfRowsInSection, and the cellForRowAt IndexPath, which is the one that, in order to make my cells behave like intended, I needed to call the super. (cellForRowAt…) and assign the cell created in the superclass as a cell variable inside this function, after obtaining the cell from my super class, I just changed the textLabel.text of the cell in question to be what’s in the categoryArray at that [indexPath.row].name (the name of the cell)

![29D653EE-0D3C-4D8E-ACB0-B748EE2CE5A2_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/851e02c0-187b-4f05-84d3-7e62339c68f0)
Part 2
Inside my data manipulation methods, I have just created two functions and overridden the updateModel function from my superclass.

The saveData function is where we access the context that was previously defined and save the data in order for it to persist throughout the app that exists inside the user's iPhone. The loadData function with request is the function that I created in order to fetch data previously saved on our CoreData database, and here the function just returns all the categories that we have inside our database.

The updateModel function is called inside our SwipeCellTableViewController, but is inside here that I gave its functionality, since I can only access the categoryArray inside this class, I used the override capability to change the behavior of the function in the superclass, providing only the path to delete the category, first inside the database and latter in our local array. 

![79F634B3-1A5E-4DE0-B976-8B972B107284_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/9f24c8e0-9b6b-4ed9-b263-1df48660ba16)
Part 3
The add new categories part of the code provides functionality for the user to create categories when the user presses the addButton inside the CategoryView, and here I just created a UIAlert by creating a UIAlertController and adding a UIAlertAction which creates and saves the category the user creates.

The code looks complex at first glance, but it is rather very simple, and every little piece of functionality can be easily viewed in Apple’s documentation of UIAlertController.

The table view delegate methods part of the code is where I defined some functionality when the user selects a specific category, and here we can see that we just prepare and perform a segue to show the user the items related to that category. One simple thing to keep in mind here is that I’ve created a variable inside the TodoListViewController called selectedCategory in order to filter the results for only that specific category.

### TodoListViewController

![ACD0BE76-96C1-4CAC-A04E-842991424065_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/80a88d71-95c9-4155-b061-45a7b8c21c39)
Part 1
The TodoListViewController is also a subclass of my previously created SwipeCellTableViewController, to allow the class’ cells to be swappable.

At first, I just created the itemArray as an array of Item objects and created the variable context in order to access the same persistentContainer.viewContext that I’ve accessed throughout the app.

Here, I also created the selectedCategory as an optional Category object, that gets set when the user selects a category, and when this occurs, the loadItems function gets called.

The table view data source methods, as expected, just implement the two most important methods to deal with tableViews, and here worth mentioning that inside the cellForRowAt indexPath method, I again, called the super. Method in order to have the ability to swipe the cells here, and just to highlight the difference from this implementation from the categoryViewController, is that here we have another attribute, which we analyze inside the data if it's true or false to show on the cell.

![BE60E802-B566-4868-AFDC-9F52832F4B37_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/1a9f0f41-d12e-4185-8000-8c49388fc06d)
Part 2
The table view delegate methods part here is very simple, as I just implemented the didSelectRowAt IndexPath in order to manipulate the done property of the Item object when the user clicks to check or to uncheck it.

The add new items part of the code was created to implement the addButtonPressed IBAction, which creates and saves inside our CoreData dataModel every item that the user creates inside this category, and since we have a relationship between our Category and Item entities, we can track for which category that item was created and what are the items associated with that category. Again, here I have used the UIAlertController with UIAlertAction.

![27CCD55E-0D74-4A0B-8CD9-4D3CE74D008C_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/847392ce-7744-4178-a8d1-55c240902eec)
Part 3
The core data manipulation methods were created to provide a way to save, load, and update the items.

The saveItems function is simple, just access the context and save its state.

The loadItems is just a little more complex than what we have seen so far because here I just want to have the capacity to filter for two things, the first one is the category, in which I used the selectedCategory variable to filter just items related to that category, and I also wanted to provide the search ability for the user.

Considering that the user can create a lot of items related to this specific category, I wanted to provide the ability to search for related items, this function considers that, if the user provides a search string, then the function behaves as expected, but in every case, the function has a filter related to the selectedCategory.

Finally, the updateModel function is just to pass the path for the data from this class to its superclass.

![A043FCF1-F1AB-49C3-A01E-FA14DAA7ABF4_1_105_c](https://github.com/guilhermemello07/todoey/assets/72673965/43aa412b-d451-46d9-8c3f-4385611d5cf0)
Part 4
The UISearchBarDelegate functionality part of the code was created to let the class adopt the UISearchBarDelegate protocol.

Here I just created two functions, the first one is the searchBarSearchButtonClicked, which just creates an NSFetchRequest, an NSPredicate, and an NSSortDescriptor in order to call the loadItems function with a request for the user to see only the items that he searched for.

The searchBar(… textDidChange…) function was created to allow all the items to appear again when the user cancels the search or when the user deletes the words inputted for the search in the searchBar.

## ⚠️Preview of the app

https://github.com/guilhermemello07/todoey/assets/72673965/f5d5aec7-497d-41e5-8eee-ce429c78c62e


## ⚠️Final thoughts

This is an academic work, and nothing here was built intended for commercial use, the sole purpose of this project is to show some knowledge of certain techniques and technologies used throughout the project.

The images used in this app, such as the logo, were created using Canva.

If you liked this project, or have something to tell me, feel free to connect through my social networks:
- [LinkedIn](https://www.linkedin.com/in/guilherme-demello/)
- [GitHub](https://github.com/guilhermemello07) 
- [Medium](https://medium.com/@guilhermemello1988)
- Instagram: @guilhermemello07 


## ⚠️Reference

The SwipeCellKit was retrieved from this [GitHub Repo](https://github.com/SwipeCellKit/SwipeCellKit) 

This project was made following the Angela-Yu iOS course which can be viewed on [Udemy:](https://www.udemy.com/course/ios-13-app-development-bootcamp/learn/lecture/10929514#overview) 








