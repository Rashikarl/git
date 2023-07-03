# git
Chrome extention with copilot to build a ..
/
Blog
EducationEngineeringProduct
How I used GitHub Copilot to build a browser extension
Here’s how, in seven steps, I built my first browser extension with GitHub Copilot—and my three major takeaways about learning and pair programming in the age of AI.

How I used GitHub Copilot to build a browser extension
Author
Rizel ScarlettRizel Scarlett
May 12, 2023
For the first time ever, I built a browser extension and did it with the help of GitHub Copilot. Here’s how.
I’ve built a rock, paper, scissors game with GitHub Copilot but never a browser extension. As a developer advocate at GitHub, I decided to put GitHub Copilot to the test, including its upcoming chat feature, and see if it could help me write an extension for Google Chrome to clear my cache.

I’m going to be honest: it wasn’t as straightforward as I expected it to be. I had a lot of questions throughout the build and had to learn new information.

But at the end of the day, I gained experience with learning an entirely new skill with a generative AI coding tool and pair programming with GitHub Copilot—and with other developers on Twitch 👥.

I wanted to create steps that anyone—even those without developer experience—could easily replicate when building this extension, or any other extension. But I also wanted to share my new takeaways after a night of pair programming with GitHub Copilot and human developers.

So, below you’ll find two sections:

🪜 How I built a Chrome extension with GitHub Copilot in seven steps.
You’ll see my questions and prompts 🧑🏾‍💻 and the responses from GitHub Copilot 💻.
🧠 Three important lessons I learned about learning and pair programming in the age of AI.
Let’s jump in.

How to build a Chrome extension with GitHub Copilot
To get started, you’ll need to have GitHub Copilot installed and open in your IDE. I also have access to an early preview of GitHub Copilot chat, which is what I used when I had a question. If you don’t have GitHub Copilot chat, sign up for the waitlist, and pair GitHub Copilot with ChatGPT for now.

1. 🧑🏾‍💻 Using the chat window, I asked GitHub Copilot, “How do I create a Chrome extension? What should the file structure look like?”
💻 GitHub Copilot gave me general steps for creating an extension—from designing the folder structure to running the project locally in Chrome.

Screenshot of the char window where the user asked GitHub Copilot "How do I build a browser extension? What should the file structure look like?" GitHub Copilot provided some instructions in response."

Then, it shared an example of a Chrome extension file structure.

Copilot response showing an example structure for a simple Chrome extension.

To save you some time, here’s a chart that briefly defines the purpose of these files:

manifest.json 🧾	Metadata about your extension, like the name and version, and permissions. Manifest as a proper noun is the name of the Google Chrome API. The latest is V3.
popup.js 🖼️	When users click on your extension icon in their Chrome toolbar, a pop-up window will appear. This file is what determines the behavior of that pop-up and contains code for handling user interactions with the pop-up window.
popup.html and style.css 🎨	These files make up the visual of your pop-up window. popup.html is the interface, including layout, structure, and content. style.css determines the way the HTML file should be displayed in the browser, including font, text color, background, etc.
2. Create the manifest.json 🧾
🧑🏾‍💻 Inside a folder in my IDE, I created a file called manifest.json. In manifest.json,

I described my desired file:

Manifest for Chrome extension that clears browser cache.
manifest_version: 3
Permissions for the extension are: storage, tabs, browsingData
I pressed enter and invoked suggestions from GitHub Copilot by typing a curly brace.

💻 Inside the curly brace, GitHub Copilot suggested the manifest. I deleted the lines describing my desired manifest.json, and the final file looked like this:

{
   "name": "Clear Cache",
   "version": "1.0",
   "manifest_version": 3,
   "description": "Clears browser cache",
   "permissions": [
       "storage",
       "tabs",
       "browsingData"
   ],
   "action": {
       "default_popup": "popup.html"
   },
   "background": {
       "service_worker": "background.js"
   }
}
3. Create a service worker, which is a file called background.js 🔧
This wasn’t a file that was recommended from my chat with GitHub Copilot. I learned that it was a necessary file from a developer who tuned into my livestream 👥. The background.js is what gives your extension the ability to run in the background, perform tasks, and respond to user events outside of the extension’s pop-up window (like network requests and data storage).

🧑🏾‍💻 In my background.js file, I wrote a comment describing my desired service worker:

Service Worker for Google Chrome Extension 
Handles when extension is installed
Handles when message is received
Then, I wrote a comment for the first function, which prompted a suggestion from GitHub Copilot, and then wrote another comment to describe the second function.

💻 The final file looked like this:

/*
Service Worker for Google Chrome Extension
Handles when extension is installed
Handles when message is received
*/

// console.log when extension is installed
chrome.runtime.onInstalled.addListener(function() {
   console.log("Extension installed");
});

// send response when message is received and console.log when message is received
chrome.runtime.onMessage.addListener(function(request, sender, sendResponse) {
   console.log("Message received");
   sendResponse("Message received");
});
4. Create the popup.html file 🖼️
🧑🏾‍💻 In the popup.html file, I wrote a comment describing how I wanted my pop-up window to appear. This window is the one users will see when they click on the extension icon.

<!--
   HTML for Chrome extension that clears browser cache.
   Connect to javascript file called popup.js and CSS file called style.css
   Will render the following buttons with id's:
   - "All History"
   - "Past Month"
   - "Past Week"
   - "Past Day"
   - "Past Hour"
   - "Past Minute"


   Will render an empty paragraph with id "lastCleared"
-->
💻 GitHub Copilot suggested the following:

<!DOCTYPE html>
<html>
   <head>
       <meta charset="utf-8">
       <title>Clear Cache</title>
       <link rel="stylesheet" href="style.css">
   </head>
   <body>
       <h1>Clear Cache</h1>
       <button id="allHistory">All History</button>
       <button id="pastMonth">Past Month</button>
       <button id="pastWeek">Past Week</button>
       <button id="pastDay">Past Day</button>
       <button id="pastHour">Past Hour</button>
       <button id="pastMinute">Past Minute</button>
       <p id="lastCleared"></p>
       <script src="popup.js"></script>
   </body>
</html>
5. Test the browser extension
I decided to test the modifications before incorporating additional styles or interactivity. I prefer making incremental changes and conducting manual tests in between because it simplifies the debugging process.

🧑🏾‍💻 Navigate to chrome://extensions/ in your Chrome browser. Turn on developer mode. Press “Load unpacked.” Upload the folder that holds your Chrome extension. Once loaded, you should be able to test your extension. Here’s what mine looked like:

Screenshot of the Chrome extension offering options to "Clear Cache."

6. Create a popup.js file to add interactivity
🧑🏾‍💻 Inside this file, write a comment that will act as pseudocode for you but as a prompt for GitHub Copilot.

Pseudocode isn’t code, but a method of conceptualizing and communicating ideas to plan and design programs before writing the actual code. Programmers can concentrate on algorithmic logic without worrying about syntactic details of a particular programming language—and communicate ideas to other developers regardless of coding experience.

Here’s the pseudocode I used:

/*
This program is a Chrome Extension that clears browser cache.
Handle on button click:
- button with id "allHistory" that clears all cache history
- button with id "pastMonth" that clears cache history from the past month
- button with id "pastWeek" that clears cache history from the past week
- button with id "pastDay" that clears cache history from the past day
- button with id "pastHour" that clears cache history from the past hour
- button with id "pastMinute" that clears cache history from the past minute


Create function that
- converts dates and times into human-readable format
- adds "Successfully cleared cache" with date and time in a paragraph with id "lastCleared"
*/
Then, write the series of comments below, but write them one at a time to allow GitHub Copilot to generate a code suggestion after each one. The final popup.js looked like this:

/*
This program is a Chrome Extension that clears browser cache.
Handle on button click:
- button with id "allHistory" that clears all cache history
- button with id "pastMonth" that clears cache history from the past month
- button with id "pastWeek" that clears cache history from the past week
- button with id "pastDay" that clears cache history from the past day
- button with id "pastHour" that clears cache history from the past hour
- button with id "pastMinute" that clears cache history from the past minute

Create function that
- converts dates and times into human-readable format
- adds "Successfully cleared cache" with date and time in a paragraph with id "lastCleared"
*/

// convert date and time into human-readable format
function convertDate(date) {
   let date = new Date(date);
   var options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
   return date.toLocaleDateString("en-US", options);
}

// add successfully cleared cache into paragraph with id "lastCleared"
function addCleared() {
   var p = document.getElementById("lastCleared");
   let date = new Date();
   p.innerHTML = "Successfully cleared cache " + convertDate(date);
}

// clear all cache history
document.getElementById("allHistory").addEventListener("click", function() {
   chrome.browsingData.removeCache({ "since": 0 }, function() {
       addCleared();
   });
});

// clear cache history from the past month
document.getElementById("pastMonth").addEventListener("click", function() {
   let date = new Date();
   date.setMonth(date.getMonth() - 1);
   chrome.browsingData.removeCache({ "since": date.getTime() }, function() {
       addCleared();
   });
});

// clear cache history from the past week
document.getElementById("pastWeek").addEventListener("click", function() {
   let date = new Date();
   date.setDate(date.getDate() - 7);
   chrome.browsingData.removeCache({ "since": date.getTime() }, function() {
       addCleared();
   });
});

// clear cache history from the past day
document.getElementById("pastDay").addEventListener("click", function() {
   let date = new Date();
   date.setDate(date.getDate() - 1);
   chrome.browsingData.removeCache({ "since": date.getTime() }, function() {
       addCleared();
   });
});

// clear cache history from the past hour
document.getElementById("pastHour").addEventListener("click", function() {
  let date = new Date();
   date.setHours(date.getHours() - 1);
   chrome.browsingData.removeCache({ "since": date.getTime() }, function() {
       addCleared();
   });
});

// clear cache history from the past minute
document.getElementById("pastMinute").addEventListener("click", function() {
  let date = new Date();
   date.setMinutes(date.getMinutes() - 1);
   chrome.browsingData.removeCache({ "since": date.getTime() }, function() {
       addCleared();
   });
});
🧑🏾‍💻 GitHub Copilot actually generated the var keyword, which is outdated. So I changed that keyword to let.

7. Create the last file in your folder: style.css
🧑🏾‍💻 Write a comment that describes the style you want for your extension. Then, type “body” and continue tabbing until GitHub Copilot suggests all the styles.

My final style.css looked like this:

/* Style the Chrome extension's popup to be wider and taller
Use accessible friendly colors and fonts
Make h1 elements legible
Highlight when buttons are hovered over
Highlight when buttons are clicked
Align buttons in a column and center them but space them out evenly
Make paragraph bold and legible
*/

body {
   background-color: #f1f1f1;
   font-family: Arial, Helvetica, sans-serif;
   font-size: 16px;
   color: #333;
   width: 400px;
   height: 400px;
}

h1 {
   font-size: 24px;
   color: #333;
   text-align: center;
}

button {
   background-color: #4CAF50;
   color: white;
   padding: 15px 32px;
   text-align: center;
   text-decoration: none;
   display: inline-block;
   font-size: 16px;
   margin: 4px 2px;
   cursor: pointer;
   border-radius: 8px;
}

button:hover {
   background-color: #45a049;
}

button:active {
   background-color: #3e8e41;
}

p {
   font-weight: bold;
   font-size: 18px;
   color: #333;
}
For detailed, step-by-step instructions, check out my Chrome extension with GitHub Copilot repo.
Three important lessons about learning and pair programming in the age of AI
Generative AI reduces the fear of making mistakes. It can be daunting to learn a new language or framework, or start a new project. The fear of not knowing where to start—or making a mistake that could take hours to debug—can be a significant barrier to getting started. I’ve been a developer for over three years, but streaming while coding makes me nervous. I sometimes focus more on people watching me code and forget the actual logic. When I conversed with GitHub Copilot, I gained reassurance that I was going in the right direction and that helped me to stay motivated and confident during the stream.
Generative AI makes it easier to learn about new subjects, but it doesn’t replace the work of learning. GitHub Copilot didn’t magically write an entire Chrome extension for me. I had to experiment with different prompts, and ask questions to GitHub Copilot, ChatGPT, Google, and developers on my livestream. To put it in perspective, it took me about 1.5 hours to do steps 1 to 5 while streaming.

But if I hadn’t used GitHub Copilot, I would’ve had to write all this code by scratch or look it up in piecemeal searches. With the AI-generated code suggestions, I was able to jump right into review and troubleshooting, so a lot of my time and energy was focused on understanding how the code worked. I still had to put in the effort to learn an entirely new skill, but I was analyzing and evaluating code more often than I was trying to learn and then remember it.

Generative AI coding tools made it easier for me to collaborate with other developers. Developers who tuned into the livestream could understand my thought process because I had to tell GitHub Copilot what I wanted it to do. By clearly communicating my intentions with the AI pair programmer, I ended up communicating them more clearly with developers on my livestream, too. That made it easy for people tuning in to become my virtual pair programmers during my livestream.

Overall, working with GitHub Copilot made my thought process and workflow more transparent. Like I said earlier, it was actually a developer on my livestream who recommended a service worker file after noticing that GitHub Copilot didn’t include it in its suggested file structure. Once I confirmed in a chat conversation with GitHub Copilot and a Google search that I needed a service worker, I used GitHub Copilot to help me write one.

Watch GitHub CEO Thomas Dohmke use GitHub Copilot to create the classic snake game in less than 18 minutes. ⤵️



Take this with you
GitHub Copilot made me more confident with learning something new and collaborating with other developers. As I said before, live coding can be nerve-wracking. (I even get nervous even when I’m just pair programming with a coworker!) But GitHub Copilot’s real-time code suggestions and corrections created a safety net, allowing me to code more confidently—and quickly— in front of a live audience. Also, because I had to clearly communicate my intentions with the AI pair programmer, I was also communicating clearly with the developers who tuned into my livestream. This made it easy to virtually collaborate with them.

The real-time interaction with GitHub Copilot and the other developers helped with catching errors, learning coding suggestions, and reinforcing my own understanding and knowledge. The result was a high-quality codebase for a browser extension.

This project is a testament to the collaborative power of human-AI interaction. The experience underscored how GitHub Copilot can be a powerful tool in boosting confidence, facilitating learning, and fostering collaboration among developers.

More resources
How to send a tweet with GitHub Copilot
8 things you didn’t know you could do with GitHub Copilot
What developers need to know about generative AI
How generative AI is changing the way developers work
A beginner’s guide to prompt engineering with GitHub Copilot
Tags:
generative AI, GitHub Copilot, tutorial
More on generative AI
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
Today at Collision Conference we unveiled breaking new research on the economic and productivity impact of generative AI–powered developer tools. The research found that the increase in developer productivity due to AI could boost global GDP by over $1.5 trillion.

Thomas Dohmke
Inside GitHub: Working with the LLMs behind GitHub Copilot
Developers behind GitHub Copilot discuss what it was like to work with OpenAI’s large language model and how it informed the development of Copilot as we know it today.

Sara Verdi
How GitHub Copilot is getting better at understanding your code
With a new Fill-in-the-Middle paradigm, GitHub engineers improved the way GitHub Copilot contextualizes your code. By continuing to develop and test advanced retrieval algorithms, they’re working on making our AI tool even more advanced.

Johan Rosenkilde
More on GitHub Copilot
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
Today at Collision Conference we unveiled breaking new research on the economic and productivity impact of generative AI–powered developer tools. The research found that the increase in developer productivity due to AI could boost global GDP by over $1.5 trillion.

Thomas Dohmke
How to use GitHub Copilot: Prompts, tips, and use cases
In this prompt guide for GitHub Copilot, two GitHub developer advocates, Rizel and Michelle, will share examples and best practices for communicating your desired results to the AI pair programmer.

Rizel Scarlett & Michelle Mannering
Inside GitHub: Working with the LLMs behind GitHub Copilot
Developers behind GitHub Copilot discuss what it was like to work with OpenAI’s large language model and how it informed the development of Copilot as we know it today.

Sara Verdi
Related posts
GitHub Enterprise Server 3.9 is now generally available
Enterprise
GitHub Enterprise Server 3.9 is now generally available
GitHub Enterprise Server 3.9 is now generally available. Organizations can now take advantage of more features that enable deeper collaboration, greater observability and faster workflows.

Melody Mileski & David Jarzebowski
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
Enterprise
The economic impact of the AI-powered developer lifecycle and lessons from GitHub Copilot
Today at Collision Conference we unveiled breaking new research on the economic and productivity impact of generative AI–powered developer tools. The research found that the increase in developer productivity due to AI could boost global GDP by over $1.5 trillion.

Thomas Dohmke
Crafting a better, faster code view
Engineering
Crafting a better, faster code view
The new GitHub Code View brings users many new features to improve the code reading and exploration experiences, and we overcame a number of unique technical hurdles in order to deliver those features without compromising performance.

Joshua Brown
Explore more from GitHub
Education
Education
Information for tomorrow’s developers.
Learn more
The ReadME Project
The ReadME Project
Stories and voices from the developer community.
Learn more
GitHub Copilot
GitHub Copilot
Your AI pair programmer.
Learn more
Work at GitHub!
Work at GitHub!
Check out our current job openings.
Learn more
Subscribe to The GitHub Insider
A newsletter for developers covering techniques, technical guides, and the latest product innovations coming from GitHub.

Your email address
Yes please, I’d like GitHub and affiliates to use my information for personalized communications, targeted advertising and campaign effectiveness. See the GitHub Privacy Statement for more details.

Subscribe
Product
Features
Security
Enterprise
Customer Stories
Pricing
Resources
Platform
Developer API
Partners
Atom
Electron
GitHub Desktop
Support
Docs
Community Forum
Training
Status
Contact
Company
About
Blog
Careers
Press
Shop
GitHub on Twitter
GitHub on Facebook
GitHub on YouTube
GitHub on Twitch
GitHub on TikTok
GitHub on LinkedIn
GitHub’s organization on GitHub
© 2023 GitHub, Inc.
Terms
Privacy
