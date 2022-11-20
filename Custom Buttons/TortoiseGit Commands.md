
[Source](https://tortoisegit.org/docs/tortoisegit/tgit-automation.html)

## Description
Common Commands to use with TortoiseGit.  TortoiseGit needs to be installed for the commands to work.


## Raw commands
The matching icons can be found in 
```
%programfiles%\TortoiseGit\bin\TortoiseGitProc.exe
```
### Show Log

```
TortoiseGitProc.exe /command:log /path:{sourcepath}
```

### Commit

```
TortoiseGitProc.exe /command:commit /path:{sourcepath}
```

### Pull

```
TortoiseGitProc.exe /command:pull /path:{sourcepath}
```

### Switch/Checkout

```
TortoiseGitProc.exe /command:switch /path:{sourcepath}
```

## Full Menu (XML)
```xml
<?xml version="1.0"?>
<button backcol="none" display="both" dropdown_glyph="yes" icon_size="large" textcol="none" type="menu">
	<label>TortoiseGit</label>
	<tip>Some usefull commands for TortoiseGit</tip>
	<icon1>/programfiles/Git/git-cmd.exe,0</icon1>
	<button backcol="none" display="both" icon_size="large" textcol="none">
		<label>Show Log</label>
		<tip>Show the log of the current location</tip>
		<icon1>/programfiles/TortoiseGit/bin/TortoiseGitProc.exe,16</icon1>
		<function type="normal">
			<instruction>TortoiseGitProc.exe /command:log /path:{sourcepath}</instruction>
		</function>
	</button>
	<button backcol="none" display="both" icon_size="large" textcol="none">
		<label>Commit</label>
		<tip>Commit changes from the current location</tip>
		<icon1>/programfiles/TortoiseGit/bin/TortoiseGitProc.exe,4</icon1>
		<function type="normal">
			<instruction>TortoiseGitProc.exe /command:commit /path:{sourcepath}</instruction>
		</function>
	</button>
	<button backcol="none" display="both" icon_size="large" textcol="none">
		<label>Fetch</label>
		<tip>Open fecth dialog to get latest changes from remote</tip>
		<icon1>/programfiles/TortoiseGit/bin/TortoiseGitProc.exe,1</icon1>
		<function type="normal">
			<instruction>TortoiseGitProc.exe /command:pull /path:{sourcepath}</instruction>
		</function>
	</button>
	<button backcol="none" display="both" icon_size="large" textcol="none">
		<label>Switch/Checkout</label>
		<tip>Switch to a different branch</tip>
		<icon1>/programfiles/TortoiseGit/bin/TortoiseGitProc.exe,9</icon1>
		<function type="normal">
			<instruction>TortoiseGitProc.exe /command:switch /path:{sourcepath}</instruction>
		</function>
	</button>
</button>
```