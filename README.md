# sublime packagecontrol
my sublimetext packagecontrol channel and github repository json files

- channels are *json* files hosted on a URL that contain a list of repository *urls*. in my case, I currently only have one repository listed - the repository published in this repo
- repositories are *json* files hosted on a URL that contain packagecontrol information about one or more sublimetext packages hosted on **github** or **bitbucket**. 
- the channel and repository *json* files are used by [packagecontrol](https://packagecontrol.io/docs/channels_and_repositories) find and access packages from their **github** or **bitbucket** repo
- in this case, the *channel.json* file is a bit of an overkill because you can easily use packagecontrol to add multiple repositories but it is convenient to just add the one *channel.json* and manage any list of repositories in that file



why host your packages on github but don't include them in **packagecontrol.io**?

- almost all of my packages are just minor tweaks of other published packages for my convenience or taste and thus aren't worthy of cluttering up the package index
- I'm untalented amateur and worse don't invest much time in quality control of these tweaks so they may be buggy

why not just develop locally in `../data/packges/user/`?

- occasionally I am asked to share despite the caveats above and this makes it easier
- practice and good order in the event I do ever develop a useful sublimetext package worth publishing and sharing more broadly


## usage

because we a using a channel we need only add the channel to **packagecontrol** in **sublimetext** and **packagecontrol** will take care of the rest. once the channel is added, any packages listed in any of the repositories (*packages.json* files) listed in the *channel.json* file will be available for installation via **packagecontrol** 

to add a channel in **packagecontrol**, (see [docs](https://packagecontrol.io/docs/usage)), `ctrl+shift+p` and use the `Package Control: Add Channel`  and paste this *url*:

```
https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/channel.json
```

`Package Control: Add Channel` (see `package_control/package_control/commands/add_channel_command.py` [packagecontrol: github](https://github.com/wbond/package_control)) just adds the *url* to the "channels": [] entry in  sublimetext's *packagcontrol.sublime-settings* file (found in ``../data/packges/user/packagcontrol.sublime-settings`) see this snippet:

```json
# snippet 
#  ..\Data\Packages\User\Package Control.sublime-settings

{
	"bootstrapped": true,
	"in_process_packages": [],
	"channels": [],                  <----- Package Control: Add Channel
	"repositories": [],              <----- Package Control: Add Repository
	"installed_packages":
	[
		"A File Icon",
		"AceJump",
		"AdvancedNewFile",
		"Alignment",
		"AlignTab",
        ...
	]
}
```



In a similar manner to Add Channel, `Package Control: Add Repository` (see `package_control/package_control/commands/add_repository_command.py` in [packagecontrol: github](https://github.com/wbond/package_control)) adds repository *urls* to the "repository":[] in  sublimetext's *packagcontrol.sublime-settings* file (found in `../data/packges/user/packagcontrol.sublime-settings`) see the above snippet. But note, because we are using a channel to manage all of the repositories we won't be using this facility.



**channel.json:**

```json
{
  "schema_version": "3.0.0",
  "repositories" : [  
    "https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/packages.json"
  ],
  "packages_cache": {},
  "dependencies_cache": {}
}
```



## repository: packages.json

here is a prototype `packages.json` file representing a repository for sublimetext packages. a file like this can be included in a `channel.json` or added directly to sublimetext using `Package Control: Add Repository`

```json
{
	// BE SURE TO REMOVE THESE COMMENTS BEFORE USING THIS TEMPLATE SINCE
	// COMMENTS ARE NOT ALLOWED IN JSON
    // source
    //   https://raw.githubusercontent.com/wbond/package_control/master/example-repository.json

	"schema_version": "3.0.0",

	// Packages can be specified with a simple URL to a GitHub or BitBucket
	// repository, but details can be overridden for every field. It is also
	// possible not utilize GitHub or BitBucket at all, but just to host your
	// packages on any server with an SSL certificate.
	"packages": [

		// This is what most packages should aim to model.
		//
		// The majority of the information about a package ("name",
		// "description", "author") are all pulled from the GitHub (or
		// BitBucket) repository info.
		//
		// If the word "sublime" exists in the repository name, the name can
		// be overridden by the "name" key.
		//
		// All packages must have one or more "releases". Releases are generated
		// from the the tags that are semantic versioning version numbers.
		//
		// A release MUST contain a "sublime_text" version selector. Use "*"
		// for all versions.
		{
			"name": "Alignment",
			"details": "https://github.com/wbond/sublime_alignment",
			"releases": [
				{
					"sublime_text": "*",
					"tags": true
				}
			]
		},

		// Here is an equivalent package being pulled from BitBucket
		{
			"name": "Alignment",
			"details": "https://bitbucket.org/wbond/sublime_alignment",
			"releases": [
				{
					"sublime_text": "*",
					"tags": true
				}
			]
		},
...
```



my `packages.json` at:

- html: `https://github.com/pjc-pub-utils/sublime-packagecontrol/packages.json`
- raw: `https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/packages.json`

```json
{
  "schema_version": "3.0.0",
  "packages": [
     {
        "name":    "AAAConEmuOpen",
        "details": "https://github.com/pjc-utils/Sublime_ConEmuOpen",
        "labels":  ["conemu", "cmd", "windows", "cmd here"],
        "releases": [
          {
            "sublime_text": ">=3000",
            "platforms": ["windows"],
            "tags": true
          }
         ]
     }
  ]
}
```



## packages

- note "tags": true means that packagecontrol pulls the highest tagged release from the repo, this is great in production but not so for testing

- there is a deprecated alternate method

  ```
  // DEPRECATED. In addition to the recommendation above of pulling
  // releases from tags that are semantic version numbers, releases can
  // also come from a branch.
  {
  	"details": "https://github.com/wbond/sublime_alignment",
  	"releases": [
          {
              "sublime_text": "*",
              "branch": "master"
          }
  	]
  }
  ```

  ​

## refs

[submitting a package to default channel (not what we are doing but relevant)](https://packagecontrol.io/docs/submitting_a_package)

## notes

- you can include the repository *url* (the raw *url* to the `packages.json` file) in the "repositories":[] key of the User Package Control.sublime-settings. I got this to work before the channel example. However, it can take a restart of sublimetext to get the cache flushed or something? 

  **Package Control.sublime-settings**

  ```
  ...
  "repositories":
  	[
  		"https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/packages.json"
  	]
  ...
  ```

  ​


- *url* references to the *json* files must be in github raw format (see [here](https://github.com/wbond/package_control/issues/815)). (as a reminder, you can get the raw *url* via the **raw** button on github after the file has been selected). *url* examples

  ```powershell
  # these DO work, github raw format
  https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/channel.json
  https://raw.githubusercontent.com/pjc-pub-utils/sublime-packagecontrol/master/packages.json

  # these DON'T work
  # the default github url is a html url and won't work 
  # because it returns the file in plain/text instead of json
  https://github.com/pjc-pub-utils/sublime-packagecontrol/master/channel.json
  ```

- *url* references to packages are in normal html format

- this is obvious, but the package repos must have public read access


