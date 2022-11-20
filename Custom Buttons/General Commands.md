## Open WindowsTerminal at Location
Fitting icon can be found [here](https://raw.githubusercontent.com/microsoft/terminal/master/res/terminal.ico)
### Raw Command

```
wt.exe -d {sourcepath}
```

### Button (XML)
```xml
<?xml version="1.0"?>
<button backcol="none" display="both" icon_size="large" label_pos="bottom" textcol="none">
	<label>Windows Terminal</label>
	<tip>Open the Windows Terminal at the current location</tip>
	<function type="normal">
		<instruction>wt.exe -d {sourcepath}</instruction>
	</function>
</button>
```