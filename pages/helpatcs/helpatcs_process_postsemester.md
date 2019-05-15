---
title: Help@CS Post-semester
keywords: postsemester, process
sidebar: helpatcs_sidebar
permalink: helpatcs_process_postsemester.html
folder: helpatcs
---

## Introduction

The section provides information on activities that should happen after each semester.

## Moodle

* Moodle content must be archived for ABET accreditation.
This is *essential* and is one of the reasons we use Moodle.
This is typically done immediately after grades are filed using
the moodle/admin/cli/backup.php script.

To make life easier you can slightly modify the following scripts in moodle to backup all course by Category (e.g. "Spring 2017")

Change moodle/backup/util/helper/backup_cron_helper.class.php

change:
```
public static function run_automated_backup($rundirective = self::RUN_ON_SCHEDULE, $CategoryID)
```

remove:
```
$rs = $DB->get_recordset(course);
```

add
```
$sql = "SELECT * from mdl_course where category=" . $CategoryID;
$rs = $DB->get_recordset_sql($sql);
```

change moodle/admin/cli/automated_backups.php
```
backup_cron_automated_helper::run_automated_backup(backup_cron_automated_helper::RUN_IMMEDIATELY, 3)
```

Add the Category ID to the arguments. In this case 3 represents 'Spring 2017'
You can determine this by querying the mdl_course_category in the DB.
e.g.  select * from mdl_course_categories;

Be sure to verify that all courses have been successfully backed up.
Once they are backed up, you need to copy these to our Amazon Storage S3
account.

The S3 folder contents looks like below so please follow this naming convention:
Important! Be sure to use AES 256 server encryption when creating the new folders.

cu-cs-moodle-backups
* 2017-fall
* 2018-spring
* 2018-summer
* 2018-fall

## Coding Environment

* Scale down the preemptive nodes
* Notify users of persistent disk cleanup
 * Remove persistent disks that are expired
