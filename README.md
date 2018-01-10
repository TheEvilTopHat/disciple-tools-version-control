# Disciple Tools Version Control
This repo hosts the public version control files necessary for triggering the remote updating of Disciple Tools.

## Steps for making a new release:
1. Update version number in `style.css`.
1. Update `version-updater.json`. (Update in two places)
1. Update `config-required-plugins.php`. 
1. Commit to GitHub.
1. Download master .zip from Github, rename .zip as `disciple-tools-theme.zip`.
1. Create a new release in GitHub with the new version number and attach `disciple-tools-theme.zip` to release.

## Example of new release block for the release notes page:
```
  <!-- New Release Block -->
	<div class="release-block">
		<div class="version-number">
			<p><strong>v0.1.3</strong></p>
		</div>
		<div class="changed-items">
			<ul style="flex: 1 0 320px; margin-bottom: 0;">
				<li>First item changed.</li>
				<li>Second item changed.</li>
			</ul>
		</div>
	</div>
	<!-- End New Release Block -->
```

## Mechanics of the updating system
If a theme is hosted in the Wordpress directory, it has native access to the updating system inside the Wordpress software. But if the theme is not hosted inside the directory, and is therefore remotely hosted, then an additional system must be used to trigger and deliver updates to the native updating system inside the Wordpress software. This is what is required for Disciple Tools, because the requirements for hosting inside the Wordpress Directory to severly limit the implementation of the Disciple Tools system. (For example, not using custom tables for which Disciple Tools requires 6.)

The system for managing the remote update in Disciple Tools uses the library `plugin-update-checker`, which is found inside `disciple-tools-theme/dt-core/libraries/`.

This library is called from a class loaded in the `functions.php` file. 
```
if ( ! class_exists( 'Puc_v4_Factory' ) ) {
            require( get_template_directory() . '/dt-core/libraries/plugin-update-checker/plugin-update-checker.php' );
        }
        Puc_v4_Factory::buildUpdateChecker(
            'https://raw.githubusercontent.com/DiscipleTools/disciple-tools-version-control/master/disciple-tools-theme-version-control.json',
            __FILE__,
            'disciple-tools-theme'
        );
```
[link](https://github.com/DiscipleTools/disciple-tools-theme/blob/0a8ea1cef2d2b168b5021cbdc103066f4f448aaf/functions.php#L214)

When the Disciple_Tools class is loaded it checkes the .json url hosted here to see if there is a version update published. If there is, then the class downloads the .zip file hosted in the releases of Disciple-Tools-Theme and pulls the `disciple-tools-theme.zip` which should be prepared as part of the release.

This file is downloaded and goes through the native updating process built into the Wordpress software.

Fortunately, Github allows for raw hosting of .json files through its [RAW file view](https://raw.githubusercontent.com/DiscipleTools/disciple-tools-version-control/master/disciple-tools-plugin-version-control.json), which makes hosting this .json version file here a great option, even thought it could be hosted on any public server.
