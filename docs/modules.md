#Modules

Adding the Zzish API is quick and easy. Here is a list of modules you can connect and utilise. They have a few things in common: they are easy to add and they create extra functionality in your app in record time. Once you have integrated with Zzish you can also use our badge to show the world there are new features to explore!

###**Not a module, but a great start - be in our App Store**

Our App Store is a collection of apps that are useful in the classrooms or as homework assignments. Teachers an browse, select and use them with a few clicks. No development effort needed, giving you a new worldwide distribution channel.  

[SUBMIT YOUR APP](https://developer.zzish.com/)

###**Teacher Dashboard**

With data flow integration you can provide a detailed and interesting insight to teachers on how their pupils are doing. This helps them in creating personalised learning plans and identify students who need more help with certain topics.
The Dashboard will show:
1. Individual pupil progress
2. Exactly what they need help in
3. Class progress
4. Average and individual scores

###**User management**

The Zzish user has been especially designed to cater to the learning market both at home and in the classroom. By bridging this gap, we are enabling learning to continue into the home.

If you have an existing user management system, you can easily integrate as we provide a field to save your domain ID so the user can be easily accessed.

We also provide a complete User management system through our learning hub (similar to Facebook login).

How it works:

We provide simple one line methods that encapsulate all the complexity in our SDK and API.
Each user has a Zzish User code that they can use to create relationships.

Dependency: Zzish API integration only

###**Recording actions - in-app assignments**
Activities are measured events. Think of a game, level, or particular challenge. There is some sort of start and end time and one or more specific user actions.

Actions represent actual user interactions. We provide some standard data inputs (eg. score, duration, correct). They feed into many other modules to provide advanced features (e.g. adaptive algorithms, learning analytics).

You can also use events to create in-app hooks that let teachers assign certain parts (levels or activities) of your app to their students. There are 2 levels of integration of this module: 
1. Create the content hooks and let teachers assign tasks, but it ends here. No data on actual progress will be collected. Students will manually have to check that they have done the task. 
2. Full integration where the task is assigned and progress is tracked. Teachers can see how long the students have been engaging with the task and what score they achieved. This has a dependency of having the Dashboard module, as all data will be available there.

###**Gamification**
Our Gamification module allows you to add multiplayer capabilities in a few hours without your own back-end server.

We have extended our Activity object to allow for multiplayer actions. You can choose to play a random opponent or a friend. The backend will perform the matching and ensure that both instances of your app have all the data it needs to present.

Dependency: Dashboard and Activities modules

###**Class management**
Teachers use the learning hub to create classrooms and add learners. The learners then login through the Zzish App Login within your App. They are able to select themselves from the class list. When the teacher is logged into the learning hub, they can monitor live progress of all the learner's interactions.
 
###**Content management**
Our Content Module will allow you to manage all of your cloud content and your individual user's content in one place. You can easily push custom/new content through our web interface.

Instead of having to redeploy with every content change or having to be online to access content we provide a content module that lets you specify sets of content that can be installed on demand from the user. For example, rather than deploying all content for all lessons with the app, you can let the user choose which lessons they wish to install. You can also use this feature to create your own content store where content is installed after learners have paid for it.

When content is updated on the server a notification is sent to devices that the content has changed and your app can update the content either automatically in the background, or notify the learner that new content is available and have the learner update the content when they wish. You can also specify whether the learner must update the content before continuing or can continue using the previous version of the content.

Our web interface is designed specifically for developers of learning apps and our server understands the concepts of lesson content, practice content and test/revision content. Practice content is content that is related directly to the lesson whereas test/revision content is additional content used to test the concepts taught in the lesson. Our server also enables you to create templated questions and answers from data.

###**App Analytics**
The App Analytics module lets you effortlessly measure both the engagement and efficacy of your learning apps. This not only lets you understand how users are engaged with your App, but also how effective the users are at learning your content. It’s like "Google Analytics for learning apps".

#####Logging Activities/Actions

The Analytics is calculated based on our core Activities and Actions. Some optional attributes are required based on your App Type.
#####Specific Customisation Based on App Type

###**Level Progression App**
If your app has levels that a user progresses through, you should have an activity for each level in your App. You should also set the "level" attribute in the activity.

###**Repeated Score Based Activities**
Each exercise should have its own activity name so that we can differ between them. No additional attributes need to be recorded.

###**Fact Based Memorization**

You should be using the ZZDeck and ZZCard Zzish objects to track the facts that you are memorising. When saving an Action set the card attribute appropriately. 
Adaptive Algorithm for fact memorising apps

Our Adaptive Algorithms module uses the spaced repetition algorithm to increase the efficiency of the learning in your app.

Simply create a deck of cards where each card represent a piece of knowledge. We inform you the next time what card should be presented to the user based on the previous results.
