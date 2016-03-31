# magic-cli
**A foundation for building your own suite of command line tools.**

magic-cli exists to make it easy to create a set of tools that work together.  It's not a tool you use as-is; it's here to offer a starting point for your own custom command line tools.

Learn more about the origins of magic-cli in [The Joy of Internal Tools](https://medium.com/@SlackEng/4a1bb5fe905b), a post on the Slack Engineering blog.

## Customization
Rename the `magic-cli` script to whatever you want — for example, if you happen to work for example.com, you might rename it to `example`. (If you want to keep any of the example commands that come with it, rename them to have that new name as a prefix — for example, rename `magic-cli-update` to `example-update`.)

Now, when you run `example`, it will look for executables in the same directory as itself which have filenames that begin with `example-`. If there's an executable called `example-build`, you could run it by typing `example build`.

Now, when you type `example`, you'll see `example build` in the list of supported subcommands. For extra credit, you can add a human-readable description in that list by putting a comment immediately under the `#!` line:

````bash
#!/usr/bin/env bash
# This line will be shown in the list of commands.
````

## Installation
This repository includes a Makefile that will install `magic-cli` and all of its subcommands into `/usr/local/bin`:

````bash
$ make install
````

You can also use it to uninstall `magic-cli`:

````bash
$ make uninstall
````

If you rename magic-cli to something different, change the value of `PREFIX` in the Makefile.

## Updates

An example script for updating the tools is also included; it makes installing the latest tools into a single step process:

```bash
$ magic-cli update
Updated tools to 01ec2ef (2016-03-30 16:20:30 -0700)
```

`magic-cli-update` provides a starting point for you to use for your own updater. If you're hosting your tools in a git repository, customizing this script just entails modifying the `SOURCE_REPO` constant at the top of the file. If your needs are more complex, you can replace the `fetch_latest_code!` and `get_version_info` methods.