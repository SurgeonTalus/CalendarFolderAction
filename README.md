# CalendarFolderAction
CalendarFolderAction

# GitHub Instructions: How to Set Up a Finder Folder Action for .ics Files

This guide will show you how to create a macOS Finder Folder Action that automatically opens `.ics` files in your specified folder and asks whether to delete them after opening. This process involves creating a folder action script using **Automator** and setting it up via the **Folder Action Setup** menu.

## Step-by-Step Guide

### 1. Create the Bash Script

First, you need to create the bash script that opens `.ics` files and prompts the user to delete them.

```bash
#!/bin/bash

# Directory to search for .ics files (no subfolders)
directory="/Users/USER/Downloads"

# Find all .ics files in the specified directory (non-recursive)
find "$directory" -maxdepth 1 -name "*.ics" | while read -r file
do
    # Open each .ics file with the default application
    open "$file"
    
    # Wait for 5 seconds
    sleep 5
    
    # Ask the user if they want to delete the file
    response=$(osascript -e 'tell app "System Events" to display dialog "Do you want to delete the file '"$file"'?" buttons {"Yes", "No"} default button "No"')
    
    # If the user clicks "Yes", delete the file
    if [[ $response == *"Yes"* ]]; then
        rm "$file"
        echo "File $file has been deleted."
    else
        echo "File $file was not deleted."
    fi
done

```
You can modify the script to your needs by changing the directory.

2. Open Automator

	1.	Open Automator from Launchpad or by searching for it using Spotlight.
	2.	Select New Document.
	3.	Choose Folder Action as the type of document.

3. Set the Target Folder

	1.	In the Folder Action template, set the folder to monitor by selecting the dropdown next to “Folder Action receives files and folders added to”.
	2.	Choose the folder where your .ics files will be downloaded (e.g., Downloads).

4. Add a “Run Shell Script” Action

	1.	In Automator, search for “Run Shell Script” in the search bar on the left.
	2.	Drag the “Run Shell Script” action into the workflow on the right.
	3.	Paste the bash script from Step 1 into the Run Shell Script text box.
	4.	Set the Shell to /bin/bash.
	5.	Set Pass input to as arguments.

5. Save the Folder Action

	1.	Click File -> Save, and give the workflow a name (e.g., ICS File Handler).

6. Set Up Folder Action in Finder

	1.	Go to the folder where you want to apply the folder action (e.g., Downloads).
	2.	Right-click on the folder and go to Services -> Folder Action Setup.
	3.	If asked, click Enable Folder Actions.
	4.	In the Folder Actions Setup window, click the + button at the bottom to add your folder.
	5.	Select the folder where .ics files are located (e.g., /Users/USER/Downloads).
	6.	In the same window, click the + button to add a folder action script.
	7.	Choose the Automator workflow you created earlier (e.g., ICS File Handler).

7. Test It Out!

Now, whenever an .ics file is added to the folder, it will automatically:

	•	Open the .ics file with the default calendar application.
	•	Wait for 5 seconds.
	•	Prompt the user with a dialog box asking whether they want to delete the file.

Additional Information

	•	If you ever want to disable or modify this action, just revisit the Folder Action Setup by right-clicking on the folder and going to Services -> Folder Action Setup.
	•	You can apply this folder action to multiple folders by adding more folders in Folder Action Setup.

This approach allows macOS to automatically handle .ics files in specific folders, streamlining your calendar file management with minimal manual intervention.





