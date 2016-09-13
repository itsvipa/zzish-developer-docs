USING THE  UNITY SDK
=========================

###**STEP 1: Download and Install the SDK**


Click here to download the SDK and complete the following instructions:

  * Inside of unity click "Asset" -> "Import New Asset" then navigate to the downloaded Zzish SDK and click "Import".
  * Then click "GameObject" -> "Create Empty" and then an empty game object should apear in your hierarchy.
  * Select the new gameobject and then click "Add Component" -> "New Script" select the language as "C Sharp" and then name it whatever.
  * Then change the script so that it extends "Zzish" like so:
     
     ~~~
     using UnityEngine;
     using System.Collections;
		
	 public class ExampleZzishManager : Zzish {
			
	 }
     ~~~
        
        
  * The SDK is now installed and ready to use.

###**STEP 2: Changing the relevant settings**


For Zzish to be able to identify your app you need to change a few settings.

  1. Inside of the ZzishSettings.cs you need to add your API key to the "API_ID" field.
  
  *Optional* Only if you would like to use Quizalize quiz content to test your application.
  
  2. Go to quizalize.com and create a quiz and then add them to a collection.
  3. Then go to the collection and select the key at the end of the URL. The URL should look like "https://www.quizalize.com/quiz/apps/966fb693-882f-480f-83f4-ac798f982186" and you want to take the ID at the end "966fb693-882f-480f-83f4-ac798f982186".
  4. Once you have the id then you add it to the "TestApID" field.
     
###**STEP 3: Integrating the SDK with your app**


  1. Firstly you need to register the user that is playing your game to a class.
	* To do this you have to call the create user function. This is done as follows:
		The fields bellow are all as follow:
		  * The UserUUID is a unique UUID for the user. This can be created by calling the V4() function.
		  * The userName is the name of the student that they can enter.
		  * The classCode is the class code that the student is given by their teacher.
		 ~~~
	    	 createUser (userUUID, userName, classCode, UserResponseHandler);
	     ~~~
	      
		  * The UserResponseHandler is a seperate function that is shown below and it handles the response from the server.
	    	 ~~~
	    	 public void UserResponseHandler (string response) {
			 	 Debug.Log (response);
			 }
	    	 ~~~
  
  2. Next make a call to get a list of the assignments like so:
         ~~~
         loadAppData (registerHandler);
         
         //Function to handle the response to the request
         public void registerHandler (string response) {
			Debug.Log (response);
		 }
         ~~~
     The string that is returned by the server will contain a list of ids for all of the quizes inside a JSON object.
  
  3. Next when a user selects a quiz from the list of assignments you can call the function below to get all of the questions from it.
		 ~~~
		 //uuid is the id of the quiz.
		 //type is weather is the type of the assignment for example a quiz is a type.
		 getQuiz (selectedQuiz.getUUID (), selectedQuiz.getType (), getQuizHander);
		 
		 //Funtion to handle the response from the server
		 public void getQuizHander (string response) {
		 	Debug.Log (response);
		 }
		 ~~~
		 
  4. Once you have the questions loaded you can start the activity.
  		 ~~~
  		 //The contentId is the id of the quiz.
  		 //The groupCode is the class code.
  		 //The groupContentId is the groupContentId found in the JSON object about the quiz.
  		 //The assignmentId is the assignmentId found in the JSON object about the quiz.
  		 //The catagoryId is the catagoryId found in the JSON object about the quiz.
  		 startActivityComplex (contentId, groupCode, "", groupContentId, assignmentId, catagoryId, numberOfQuestions,quizName,startActitivtyCallback);
  		 
  		 public void startActitivtyCallback (string message) {
		 	Debug.Log (message);
	     }
  		 ~~~
  5. Then once the game has started you can report the result of the questions allowing teachers to see each students progress on the Zzish dashboard.
  	     ~~~
  	     //V4() is just a random uuid for the action
  	     //question is the question that was asked
  	     //userAnswer is the answer that the student gave
  	     //reward is the number of points you want to award the student
  	     //duration was the amount of time it took to answer the question
  	     //correct is a boolean of either true or false
  	     //actionState is the currenta actionState as a JSON onject.
  	     //answer is the correct answer to the question
  	     LogActivity (V4 (), question, userAnswer, reward, duration, attempts, correct, actionState, answer, CompleteActivityHandler);
  	     
  	     //A funtion to handle the server response
  	     public void CompleteActivityHandler (string response) {
		 	Debug.Log (response);
		 }
  	     ~~~
  6. Upon completion of the activity you must call the CompleteActivity function like so.
  		 ~~~
  		 CompleteActivity (CompleteActivityHandler);
  		 
  		 public void CompleteActivityHandler (string response) {
		 	Debug.Log (response);
		 }
  		 ~~~
  		 
  7. If the activity is stopped for any reason whilst it is running you must call.
  		 ~~~
  		 CancelActivity (CancelActivityHandler);
  		 
  		 public void CancelActivityHandler (string response) {
			Debug.Log (response);
		 }
  		 ~~~
  		 
*NOTE: If you would like to include your own content instead of using content from Zzish simply skip out steps 2 and 3.*
