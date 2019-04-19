---
title: Moodle
keywords: procedure, moodle
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_moodle.html
folder: helpatcs
---

## Backup/Restore/Copy Course

One of the more common Moodle-related admin tasks is creating new
courses at the start of each semester.

Sometimes instructors will request blank courses. To do this:

1. Go to Admin -> Courses -> Manage Courses & Categories,
2. Navigate to the category for the current semester
3. Select create course and fill in the information (title, short name, start date).
* The start date should be the starting date of the semester since all dates in the course
are keyed off of that start.
* The course short name should be of the form CSCInnnn-semYY where 'sem' is F, S or Su and YY is the year digits.
* The course title should be of the form CSCInnnn - faculty - title.
4. When the course is created, navigate to the course, and add the instructor as 'teacher'. They can then add other teachers (TA's, etc).

More often than not, faculty want a copy of a previous semester's course. To create such a copy, one must first backup the course using Moodle built-in course backup features, and then restore the course to the new semester. You can either use a course from the S3 archive or from prior semesters. To restore from a prior semester that is still on Moodle:

1. Navigate to the class instance
2. Go to Backup in the course admin menu
3. Backup the course
4. Select the "restore" option.

You would restore the course as a new course in the new semester category. When you restore the course, insure that you're
not also restoring the users. Once restored, modify the title and the short name to match the appropriate faculty and semester.

## Backup & Archiving Course


We configured Google Compute to make nightly snapshots of the SQL database  
and Moodle data disk going back 60 days. You can view those snapshots in the project level
dashboard (Compute Engine -> Snapshots).

In the event of total failure. Here are the steps to restore the data.

1. Bring down all k8s cluster. You cannot restore disks that are mounted.
2. Create a new disk.
3. Specify name (note, you'll need to delete the old disk before, so do this carefully. Maybe test first with a different name)
4. Build the disk from snapshot and select what date you want to backup from
5. Run Create.

To restore the SQL data, the steps are even easier.

1. Go to SQL dashboard in GCE.
2. Go to backups
3. Right click on backup and click restore and select target instance. Note, you will need to remove the High Availability before GCE will let you do this.
4. Backup

Once the data has been restored, you can redeploy the k8s environment.

It is essential to archive all Moodle course content at the end of
each semester. Moodle is used for ABET accreditation and the full
backups (including user information) are necessary for that.

Current backups are archived to [Amazon Glacier](https://aws.amazon.com/glacier/) using the departmental AWS account.
Backups can be done by logging into Moodle, using moodle/admin/cli/backup.php to produce MBZ files and then transferring those to the S3 bucket labeled "moodle-backups". Archives are stored by year and semester.

The backup.php requires knowing the course id number for the course.
These can be determined using the Moosh utility (see moosh.org) and
the course-list command. Building complete backups takes a while (couple
of hours)

For more details, the Moodle backup and restore docs can be found at:
+ https://docs.moodle.org/en/Backup

## Moosh Install

1. Download the version of Moosh that matches the version of Moodle in use- [Moosh Utilities](https://moodle.org/plugins/pluginversions.php?id=522)
2. Extract the ZIP file
```
unzip moosh_moodleVV_YYYYMMDDxy.zip
```
3. Tar the output of the zip file (Containers do not have unzip support)
```
tar -cvzf moosh.tgz moosh
```
4. Find a php-fpm pod to work with
```
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php-fpm
```
4. Copy the tar file to a php-fpm pod
```
kubectl cp moosh.tgz php-fpm-XXXXXXXX-XXXX:/srv/moodle/
```

5. Connect to the containter
```
kubectl exec -it php-fpm-XXXXXXXX-XXXX /bin/bash
```

6. Extract Moosh
```
cd /srv/moodle
gunzip moosh.tgz
tar -xvf moosh.tar
rm moosh.tar
```

## Manual Course Backup & Restore

There are large courses in Moodle that cannot be completed using the web interface. Attempting to Backup one of these courses will result in the Backup Complete page not being shown. Instead you will be presented with a blank content page or a progress bar that does not progress. When this occurs, you will need to use the [Moosh](https://moosh-online.com/) at the command line to process the backup and restore.

{% include note.html content="Moosh is used because Moodle only offers a CLI backup, and not a Restore." %}

{% include note.html content="Before starting this process you must know the course ID you wish to backup. This can be found by going to the course page in Moodle and the URL will show '...?id=XXXX'" %}


1. Login to a php-fpm pod
```
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php-fpm
kubectl exec -it php-fpm-XXXXXXXX-XXXX /bin/bash
```

2. Backup the course by COURSE_ID
```
cd /srv/moodle/code
su -s /bin/bash -c "/srv/moodle/moosh/moosh.php course-backup -f /tmp/[COURSE_ID].mbz [COURSE_ID]" www-data
```

3. Determine the Category ID where the course will be restored
```
su -s /bin/bash -c "/srv/moodle/moosh/moosh.php category-list" www-data
```

4. Restore the course
```
su -s /bin/bash -c "/srv/moodle/moosh/moosh.php course-restore /tmp/[COURSE_ID].mbz [CATEGORY_ID]" www-data
```

5. Clean up
```
rm /tmp/[COURSE_ID].mbz
```

5. Open the Moodle web page and Login
6. Navigate to the new course
7. Fix the Name, Short Name, Start Date
8. Navigate to the course Participants
9. Select all Participants and delete them
10. Enroll the new teacher as a Teacher in the class
11. Send an email to the teacher with a link to the course page
