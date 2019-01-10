The console history is a record of everything that is executed in the console. so every time you're entering the 200k you are appending that to the history. LocalStorage space cant be adjusted its set to 5MB, but as long as your entry is under that and you are just entering it once then this shouldn't be a problem.

You can delete the console history to free up space.
The devtools is just another window so it can be inspected and modified like any other webpage.

Open Dev Tools

Select More Tools > Developer Tools from the Chrome Menu.
or

Right-click on a page element and selecting "inspect element" in the context menu
or

Use Ctrl/Cmd+Shift+I

Inspect Dev Tools window.

In another tab/window navigate to chrome://inspect/#other.
Locate the page which starts with "chrome-devtools://devtools/bundled/"
Click the relevant inspect link.
This will open another devtools window where you can inspect and modify the devtools.

Remove consoleHistory form LocalStorage

Navigate to the newly opened devtools window.

Unfortunately typing localStorage.removeItem('consoleHistory'); into the console doesn't work;

So you have to do it the long way.

Open "Resources" panel
Expand "Local Storage" either by double clicking "Local Storage" or click the arrow.
Select the called "devtools://devtools" table. (As its full, it may take a while to open)
locate and select the key called 'consoleHistory'
Hit delete or the "X" button.
Note. This answer has been updated to apply to version 46.0.2490.86. some details my have changed since posting. Please leave a comment if this method no longer applies.

If you are using an older version of chrome and this does not work for you then there is an additional few steps you have to take before using this method. You can find them here .

you can find your version at chrome://chrome/
