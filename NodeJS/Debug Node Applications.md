# How do I debug Node.js applications?

We can use following ways to debug node applications
1. npm install -g node-inspector
    node-debug app.js   Run this command to start debug mode in node
2. The V8 debugger released as part of the Google Chrome Developer Tools can be used to debug Node.js scripts. A detailed explanation of how this works can be found in the Node.js GitHub wiki.https://github.com/nodejs/node/wiki/Using-Eclipse-as-Node-Applications-Debugger
3. Node has its own built in GUI debugger as of version 6.3 (using Chrome's DevTools) Simply pass the inspector flag and you'll be provided with a URL to the inspector:
node --inspect server.js
You can also break on the first line by passing --inspect-brk instead.
To open a Chrome window automatically, use the inspect-process module.
### install inspect-process globally
npm install -g inspect-process
### start the debugger with inspect
inspect script.js
4. Node.js version 0.3.4+ has built-in debugging support.(http://nodejs.org/api/debugger.html)
    node debug script.js
5.Theseus(https://github.com/adobe-research/theseus) is a project by Adobe research which lets you debug your Node.js code in their Open Source editor Brackets. It has some interesting features like real-time code coverage, retroactive inspection, asynchronous call
6.For some useful articles, check out

RisingStack - Debugging Node.js Applications
Excellent article by David Mark Clements of nearForm
If you feel like watching a video(s) then

Netflix JS Talks - Debugging Node.js in Production
Interesting video from the tracing working group on tracing and debugging node.js
Really informative 15-minute video on node-inspector
Whatever path you choose, just be sure you understand how you are debugging
