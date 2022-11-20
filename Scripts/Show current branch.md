## Details

### Author
chaoticbob (modified by Leo)

### Source
[Source](https://resource.dopus.com/t/showing-git-branch-in-dopus/18347/8)

### Effect

This script will display the current branch name along side the current folder name in the tab and in the window title.

### Usage
Add a new script via *Prefernces > Toolsbars > Scripts* and name it *CurrentBranch*. Select *JScript* as Script Langauge.

Then copy and paste the script below and restart DOpus.

## Script
```js
function OnInit(initData) {
	initData.name = "Git Branch Title";
	initData.version = "1.0";
	//	initData.copyright = "...";
	initData.url =
		"https://resource.dopus.com/t/showing-git-branch-in-dopus/18347";
	initData.desc = "Show Git branch in window title";
	initData.default_enable = true;
	initData.min_version = "12.0";
}

function FindGitHead(path) {
	var git_head_path = null;
	if (path.drive != 0) {
		// Leo 26/Jun/2018: Only check on local drives.
		var n = path.components;
		for (var i = 0; i < n; ++i) {
			var file_path = DOpus.FSUtil.NewPath(path);
			file_path.Add(".git");
			file_path.Add("HEAD");
			if (DOpus.FSUtil.Exists(file_path)) {
				git_head_path = DOpus.FSUtil.NewPath(file_path);
				break;
			}
			path.Parent();
		}
	}
	return git_head_path;
}

function ReatGitBranch(path) {
	var git_branch = null;
	var fso = new ActiveXObject("Scripting.FilesystemObject");
	var file = fso.OpenTextFile(path, 1, -2);
	while (!file.AtEndOfStream) {
		var line = file.ReadLine();
		var re = /ref: refs\/heads\/(.+)/;
		var match = re.exec(line);
		if (match != null) {
			git_branch = match[1];
		}
	}
	file.Close();
	return git_branch;
}

function UpdateTitleAndLabel(tab) {
	var label = tab.path.filepart;
	var git_head_path = FindGitHead(tab.path);
	if (git_head_path != null) {
		var git_branch = ReatGitBranch(git_head_path);
		if (git_branch != null) {
			label = label + " (" + git_branch + ")";
		}
	}
	// New DOpus command
	var dopus_cmd = DOpus.NewCommand();
	// Change lister title
	var cmd = 'SET LISTERTITLE="' + label + '"';
	dopus_cmd.RunCommand(cmd);
	// Change tab label
	var cmd = 'GO TABNAME="' + label + '"';
	dopus_cmd.RunCommand(cmd);
}

function OnActivateTab(data) {
	UpdateTitleAndLabel(data.newtab);
}

function OnAfterFolderChange(data) {
	if (data.result) {
		UpdateTitleAndLabel(data.tab);
	}
}
```