## Details

### Author
Alexander Pahn

### Source
[Source Dynamic Git Actions Toolbar](./Dynamic%20Git%20Actions%20Toolbar)
[Source Show Current Branch](Show%20Current%20Branch.md)

### Effect

A set of Git related scripts. This is a combination of DynamicGitActionsToolbar and Git Branch Title to share functionality and to avoid duplicate code and code execution.

**This script will...**
- disable or enable a toolbar if the currently viewed folder is a Git repository
- show the current branch name in the tab and window title

See sources above for more details on each script.

By default the toolbar name is *GitActions* and it will be placed at the top in line 2, 230 pixels to the right. These values can be configured through the 3 corresponding variables.

### Usage
Add a new script via *Prefernces > Toolsbars > Scripts* and name it *DynamicGitActionsToolbar*. Select *JScript* as Script Langauge.

Then copy and paste the script below and restart DOpus.

## Script
```JavaScript
// GitPowerScripts
// (c) 2022 Alexander Pahn

// This is a script for Directory Opus.
// See https://www.gpsoft.com.au/DScripts/redirect.asp?page=scripts for development information.

// Called by Directory Opus to initialize the script
function OnInit(initData) {
	initData.name = "GitPowerScripts";
	initData.version = "1.0.0.0";
	initData.copyright = "(c) 2022 Alexander Pahn";
	initData.url = "https://resource.dopus.com/c/buttons-scripts/16";
	initData.desc = "A set of Git related scripts. This is a combination of DynamicGitActionsToolbar and Git Branch Title to share functionality";
	initData.default_enable = true;
	initData.min_version = "12.0";
}

// Configuration Details can be found here: https://www.gpsoft.com.au/help/opus12/index.html#!Documents/Toolbar.html
var toolbarName = "GitActions";
var position = "top";
var lineAndPosition = "2,230";

// Event Handlers
function OnActivateTab(data) {
	UpdateGitInfos(data.newtab);
}

function OnAfterFolderChange(data) {
	if (!data.result) {
		return;
	}
	UpdateGitInfos(data.tab);
}

// Wrapper Function to update Git infos
function UpdateGitInfos(tab) {
	// Create a command object
	var commandObj = DOpus.NewCommand();

	// Get Git infos
	var gitHeadPath = FindGitHead(tab.path);
	var isGitRepo = gitHeadPath != null;

	// Add commands
	AddToolbarStateCommand(isGitRepo, commandObj);
	AddTabAndTitleCommand(tab, gitHeadPath, commandObj);

	// Run all commands
	commandObj.Run();
}

function AddToolbarStateCommand(isGitRepo, commandObj) {
	// Create open and close commands dynamically
	var openCommand = "STATE=" + position + " LINE " + lineAndPosition;
	var closeCommand = "CLOSE";

	// Change toolbar state command
	var cmd = "Toolbar " + toolbarName + " " + (isGitRepo ? openCommand : closeCommand);

	// Add command to the command object
	commandObj.AddLine(cmd);
}

function AddTabAndTitleCommand(tab, gitHeadPath, commandObj) {
	var label = tab.path.filepart;

	// Try to find the current branch name
	var branchName = ReadGitBranch(gitHeadPath);

	if (branchName != null) {
		// Append label with branch name
		label = label + " (" + branchName + ")";
	}

	// Create rename commands
	var listerCmd = 'SET LISTERTITLE="' + label + '"';
	var tabCmd = 'GO TABNAME="' + label + '"';

	// Add commands to the command object
	commandObj.AddLine(listerCmd);
	commandObj.AddLine(tabCmd);
}

function FindGitHead(path) {
	var gitHeadPath = null;
	if (path.drive == 0) {
		return null;
	}
	var n = path.components;
	for (var i = 0; i < n; ++i) {
		var file_path = DOpus.FSUtil.NewPath(path);
		file_path.Add(".git");
		file_path.Add("HEAD");
		if (DOpus.FSUtil.Exists(file_path)) {
			gitHeadPath = DOpus.FSUtil.NewPath(file_path);
			break;
		}
		path.Parent();
	}

	return gitHeadPath;
}

function ReadGitBranch(path) {
	if (path == null) {
		return null;
	}
	var branchName;
	var fso = new ActiveXObject("Scripting.FilesystemObject");
	var file = fso.OpenTextFile(path, 1, -2);

	while (!file.AtEndOfStream) {
		var line = file.ReadLine();
		var re = /ref: refs\/heads\/(.+)/;
		var match = re.exec(line);
		if (match != null) {
			branchName = match[1];
		}
	}

	file.Close();
	return branchName;
}

```