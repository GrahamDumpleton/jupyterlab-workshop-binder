# jupyterlab-workshop launcher for Binder

This repository builds a [mybinder.org](https://mybinder.org) image with
JupyterLab and the
[jupyterlab-workshop](https://github.com/GrahamDumpleton/jupyterlab-workshop)
extension, and nothing else. It holds no workshops of its own: the link
that starts it says which workshop, collection or catalog to open, so one
image serves any workshop published as a gist or a git repository, without
a Binder build of its own.

## Launch links

mybinder's `urlpath` parameter names the page JupyterLab opens once the
session has started. Give it a jupyterlab-workshop launch link, URL
encoded:

```
https://mybinder.org/v2/gh/GrahamDumpleton/jupyterlab-workshop-binder/main?urlpath=<encoded launch link>
```

For a workshop published as a gist, the launch link is
`lab?workshop=https://gist.github.com/<owner>/<id>`:

```
https://mybinder.org/v2/gh/GrahamDumpleton/jupyterlab-workshop-binder/main?urlpath=lab%3Fworkshop%3Dhttps%3A%2F%2Fgist.github.com%2F%3Cowner%3E%2F%3Cid%3E
```

For a workshop in a directory of a git repository, add `subdir` (and `ref`
for a branch or tag other than the default):

```
https://mybinder.org/v2/gh/GrahamDumpleton/jupyterlab-workshop-binder/main?urlpath=lab%3Fworkshop%3Dhttps%3A%2F%2Fgithub.com%2FGrahamDumpleton%2Fjupyterlab-workshop%26subdir%3Dexamples%2Fhello-jupyterlab
```

`lab?collection=<url>` and `lab?catalog=<url>` work the same way and open
the workshop browser on what they list. The other launch link parameters,
such as `var.<name>=<value>` and `restart=force`, pass through too; see
[launch links](https://jupyterlab-workshop.readthedocs.io/en/latest/collections.html#launch-links)
in the documentation.

Arriving without a launch link starts in the workshop browser, empty,
where a workshop can be added from its URL.

## What the image does

- `binder/requirements.txt` installs JupyterLab and a pinned release of
  jupyterlab-workshop. mybinder caches the image it builds for a commit,
  so the pin is bumped after each release to bring the launcher up to
  date.

- `binder/runtime.txt` selects a Python the package supports.

- `binder/postBuild` writes a settings override that starts in the
  workshop browser and turns off JupyterLab's news question, then removes
  this repository's own files from the home directory, so the visitor's
  file browser holds only the workshops they open.

Nothing is trusted in advance: the content is whoever's the link points
at, so the trust dialog asks before a workshop's actions run, as it would
in any other JupyterLab.
