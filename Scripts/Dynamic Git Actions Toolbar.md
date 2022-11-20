## Details

### Author
Alexander Pahn

### Source
[Source](https://github.com/Hollow-Ego/vault-dopus)

### Effect

This script will disable or enable a toolbar if the currently viewed folder is a Git repository.

By default the toolbar name is *GitActions* and it will be placed at the top in line 2, 230 pixels to the right. These values can be configured through the 3 corresponding variables.

### Usage
Add a new script via *Prefernces > Toolsbars > Scripts* and name it *DynamicGitActionsToolbar*. Select *JScript* as Script Langauge.

Then copy and paste the script below and restart DOpus.

## Script
```JavaScript
// DynamicGitActionsToolbar
// (c) 2022 Alexander Pahn

// This is a script for Directory Opus.
// See https://www.gpsoft.com.au/DScripts/redirect.asp?page=scripts for development information.

// Called by Directory Opus to initialize the script
function OnInit(initData) {
	initData.name = "DynamicGitActionsToolbar";
	initData.version = "1.0.0.0";
	initData.copyright = "(c) 2022 Alexander Pahn";
	initData.url = "https://resource.dopus.com/c/buttons-scripts/16";
	initData.desc =	"Enable/Disables GitActionsToolbar based on if the current source is a git repo.";
	initData.default_enable = true;
	initData.min_version = "12.0";
}
// Configuration Details can be found here: https://www.gpsoft.com.au/help/opus12/index.html#!Documents/Toolbar.html
var toolbarName = "GitActions";
var position = "top";
var lineAndPosition = "2,230";

function HasGitHead(path) {
	// Only check on local drives
	if (path.drive == 0) {
		return false;
	}

	var n = path.components;
	for (var i = 0; i < n; ++i) {
		var file_path = DOpus.FSUtil.NewPath(path);
		file_path.Add(".git");
		file_path.Add("HEAD");
		if (DOpus.FSUtil.Exists(file_path)) {
			return true;
		}
		path.Parent();
	}

	return false;
}

function HandleToolbarState(tab) {
	// Check if it is a Git Repository
	var isGitRepo = HasGitHead(tab.path);

	// Create open and close commands dynamically
	var openCommand = "STATE=" + position + " LINE " + lineAndPosition;
	var closeCommand = "CLOSE";
	
	// New DOpus command
	var dopusCmd = DOpus.NewCommand();

	// Change toolbar state
	var cmd = "Toolbar " + toolbarName + " " + (isGitRepo ? openCommand : closeCommand);
	dopusCmd.RunCommand(cmd);
}

function OnActivateTab(data) {
	HandleToolbarState(data.newtab);
}

function OnAfterFolderChange(data) {
	if (!data.result) {
		return;
	}
	HandleToolbarState(data.tab);
}

```