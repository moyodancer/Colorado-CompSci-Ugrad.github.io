---
title: H5P
keywords: moodle, h5p
sidebar: moodle_sidebar
permalink: moodle_h5p.html
folder: moodle
---

## Introduction

The H5P activity module enables you to create interactive content such as Interactive Videos, Question Sets, Drag and Drop Questions, Multi-Choice Questions, Presentations and much more.

In addition to being an authoring tool for rich content, H5P enables you to import and export H5P files for effective reuse and sharing of content.

User interactions and scores are tracked using xAPI and are available through the Moodle Gradebook.

You add interactive H5P content by creating content using the built-in authoring tool or uploading H5P files found on other H5P enabled sites.

## Backup/Restore perforamnce
You may experience a very slow backup and restore of the course contains H5P content. The backup script of H5P will dump every library installed during the backup process, which impact on the backup performance.

You can exclude the libraries from the backup by adding the configuration option to Moodle's config.php
```
$CFG->mod_hvp_backup_libraries = '0';
```

## Links

[https://h5p.org/interactive-video](https://h5p.org/interactive-video)

[https://h5p.org/](https://h5p.org/interactive-video)

## Moodle Demo


 [Overview of Interactive Content](https://moodle.cs.colorado.edu/mod/hvp/view.php?id=29341)
 
 [Overview of probability Interactive Content](https://moodle.cs.colorado.edu/mod/hvp/view.php?id=29342)
