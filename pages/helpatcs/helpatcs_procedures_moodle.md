---
title: Moodle
keywords: procedure, moodle
sidebar: helpatcs_sidebar
permalink: helpatcs_procedures_moodle.html
folder: helpatcs
---

## Backup/Restore/Copy Course via the Web Interface

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

## Initial Command Line Environment Setup

```
gcloud auth application-default login
gcloud config set project emerald-agility-749
gcloud container clusters list
gcloud container clusters get-credentials production --zone us-west1-a
```


## Restore from backup after system failure

Google Compute is configured to make nightly snapshots of the SQL database  
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
gcloud config set project emerald-agility-749
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
gcloud config set project emerald-agility-749
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

## Problem: Email not sending

From time to time, Moodle is unable to send out the regular email notifications. To confirm the problem:
1. Navigate to Moodle -> Site Administration -> Server -> Email Test
2. Provide a "To email address"
3. Click "Send a test message"

If the next page is not Success, follow the steps below to restart the SMTP pod.
1. Identify the SMTP pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep smtp
```
2. Kill the existing pod
```
kubectl delete pod [SMTP pod name]
```
3. Wait for the new pod to reach Running state
```
kubectl get pod | grep smtp
```
4. Resend the test email as described above to confirm it is working again


## Deploy Moodle to DEV

This build process is dependant on the following utilities being installed
* gcloud kubectl (this part of gcloud components)
* envsubst (brew install gettext)
* realpath (brew tap iveney/mocha && brew install realpath)

1. Connect to Development
```
gcloud config set project csel-dev-161517
gcloud config list
kubectl config use-context gke_csel-dev-161517_us-west1-a_development
kubectl config set-context gke_csel-dev-161517_us-west1-a_development --namespace development
```


2. Restore the SQL development database with production data in the [Cloud Console](https://console.cloud.google.com).
    1. Click SQL from the left side navigation
    2. Click moodle-mysql-dev from the content area
    3. Click the Start button to start the dev instance
    4. Click Back to return to the SQL server list
    5. Click moodle-mysql from the content area
    6. Click the Backups tab
    7. Find the most recent backup and click on the 3-dot button to its right
    8. From the drop-down menu, click Restore
    9. Set the Target Instance drop-down to "moodle-mysql-dev"
    10. Click OK

3. Restore the Moodle data disk.
* This process is not ideal but Google Compute does not allow you to access snapshot disks across projects, however, it does allow you to share images. So we first create an image from a snapshot and share this with the development project.
    1. Create an Image of the most recent moodle-data-disk-ssd
        1. Click the project name at the top of the page
        2. In the Select From drop-down, select No organization
        3. Click CSEL from the project list
        4. Click Compute Engine from the left side navigation
        5. Click Images from the left side navigation
        6. Create Create Image from the top tool bar
        7. Name the image moodle-data-disk-ssd-image-YYYYMMDD
        8. From the Source drop-down, select Snapshot
        9. In the Source snapshot drop-down, select the gcs-moodle-data-disk-ssd-moodle-data-XXXXXX with the largest number
        10. Click the Create button at the bottom of the form
        11. Wait for the image to be created
    2. Find the selflink path for the new image
        1. Click Compute Engine from the left side navigation
        2. Click Images from the left side navigation
        3. Click on the name of the image from step 1-4
        4. Click Equivalent REST at the bottom of the page
        5. Find the "selfLink" value and copy it for use in creating the new disk
    3. Delete the existing moodle-data-disk-ssd in Development
		    1. Click the project name at the top of the page
        2. In the Select From drop-down, select colorado.edu
        3. Click CSEL-DEV from the project list
        4. From the left navigation, Click Disks (under Compute Engine)
        5. **Double check that you are in the CSEL-DEV project**
        6. Find the moodle-data-disk-ssd
        7. Click the 3-dot button to the right
        8. Click Delete from the drop-down menu
				9. **Triple check that you are in the CSEL-DEV project**
				10. Click the Delete button
    4. Create a new moodle-data-disk-ssd in development.
        1. Run the same window you preformed step 1, run the following command after replacing [SELFLINK_PATH] with the selflink path captured when creating the new image
        2. ```gcloud compute disks create moodle-data-disk-ssd --zone=us-west1-a --image=[SELFLINK_PATH]```

4. Determine the stable version of Moodle for the upgrade
[https://download.moodle.org/releases/latest/](https://download.moodle.org/releases/latest/)

5. Clone the Moodle repo (or pull updates if you already have it)
```
git clone git@bitbucket.org:ucbcsops/k8s-moodle.git
```

6. Clone the Registry (or pull update if you already have it)
```
git clone git@bitbucket.org:ucbcsops/registry.git
```

7. Edit the develop variables file
```
cd k8s-moodle
vi environments/development/variables.conf
```

8. Change the JENKINS_BUILD variable to match the desired version number and set the build number
Note: Change version number to match what you found in step 3 and the revision number (-1) should match the build number in Jenkins.
```
export JENKINS_BUILD=development3.6.3-1
```

9. Push the change to bitbucket
```
git add environments/development/variables.conf
git commit -m "Update Jenkins build number"
git push
cd ..
```

10. Create a development branch of the registry
Note: Change version number to match
```
cd registry
git checkout -b development3.6.3
```


11. Edit the Moodle-base Dockerfile
```
vi moodle-base/Dockerfile
```

12. Change the Moodle version to match the desired version number
Note: Change version number to match what you found in step #3
```
ENV MOODLE_VERSION v3.6.3
```

13. Download the matching (or most recent) REMUI theme (as needed) to registry/moodle-ucbcs/theme
[EdWiser](https://edwiser.org/my-account/)

14. Change into the theme directory
```
cd moodle-ucbcs/theme
```

15. Unzip the new Theme
```
unzip Edwiser_Remui_3_443e8cd7b289e3d44.zip
```

16. Unzip each of the nested Zip files
```
unzip Edwiser_Course_Formats_v1.0.0.zip
unzip edwiser_remui_3.6.2.zip
unzip edwiser_remui_block_1.0.3.zip
```

17. Move each into a versioned folder
Note: REMUI 3.6.2 was the latest version when this was written
```
mv remui remui_3.6.2
mv remuiblck remuiblck_3.6.2
mv remuiformat remuiformat_3.6.2
```

18. Cleanup zip files
```
rm *.zip
```

19. Force hiding of the local login option
```
vi remui_3.6.2/classes/output/core_renderer.php
```

20. Find the login box text
```
// sign in popup
```

21. Add the style the form tag
```html
style="display: none"
```

22. Edit the Moodle-ucbcs header fragment
```
vi ../fragments/0-header.fragment
```

23. Modify the theme paths
```
ADD theme/remui_3.6.2 ${MOODLE_CODE_DIR}/theme/remui
ADD theme/remuiblck_3.6.2 ${MOODLE_CODE_DIR}/blocks/remuiblck
ADD theme/remuiformat_3.6.2 ${MOODLE_CODE_DIR}/course/format/remuiformat
```

24. Push branch to repo
```
cd ../..
git add .
git commit -m "Moodle DEV upgrade"
git push --set-upstream origin development3.6.2
```

25. Change into the k8s-moodle directory
```
cd ../../../k8s-moodle
```

26. Fix permissions on scripts
```
chmod 750 build/development/namespace/*
chmod 750 build/development/scripts/*
```


27. Check [Jenkins](https://ci.csel.io/) for build completion (Must be on CU VPN)


28. Tear down all services in DEV
```
build/development/scripts/tear-down-all
```
Note: a remaining cluster node is okay

29. Ensure all pods are gone
```
kubectl get pod
```

30. Build the environment
```
./build-env development
```

31. Verify environment data
```
> [1] stage: load environment data
>        - checking for environment folder at environments/development...
>        - variables file found, attempting to load it...
>        - all variables successfully loaded:
>
>          - environment hostname: moodle-dev.csel.io
>          - container registry  : gcr.io/csel-dev-161517
>          - load balancer addr  : 35.185.192.128
>          - k8s target namespace: development`
```

32. Verify NFS IP is sane
```
> [4] stage: create NFS service to obtain IP
>       - templating the NFS service configuration file only...
>       - single file created at build/development/workloads/nfs/svc.yaml
>       - creating the NFS server service...
>         - kubectl: service "nfs-server" created
>       - attempting to obtain the NFS service IP address...
>         - service address: 10.43.250.93
```

33. Deploy the environment
```
build/development/scripts/deploy-all
```

Error messages about NFS already existing are okay/expected.

34. Test (Must be on CU VPN)
[Moodle-dev](https://moodle-dev.csel.io)

Remember: When you are done Testing, stop all services and **shutdown the moodle-mysql-dev database**

### Additional Documentation Link(s)

[https://bitbucket.org/ucbcsops/k8s-moodle/src/master/](https://bitbucket.org/ucbcsops/k8s-moodle/src/master/)


## Moodle DEV testing checklist
* QA Test
* NFS Data Mount
* Run Benchmark
* Code Runner
* Course Content (Y)
* Qngine
* Quiz Content
* Shib Login/Logout
* Manual Login/Logout
* Course Category
* Course Page
* Grades
* Participants
* Enroll Users
* Dashboard
* LTI
* STACK
* H5P
* Turn Editing On (Teacher)
* Calendar
* Change Roles
* Add Resource in Course
* Site Admin Console
* Memchached (Clear)
* PHP Config Lib
* Email
* Create Test Course
* Backup Course
* Restore Course

## Backup Moodle courses

{% include note.html content="This process can required a significant amount of disk space. If insufficient disk space exists, the process may need to be deconstructed to complete the backups in stages instead of a single batch. If you do run into storage limitations, be sure to increase the size of the Moodle disk at the next upgrade window." %}

1. Login to Amazon S3
 * [Amazon Login](https://us-east-2.console.aws.amazon.com/console/home?region=us-east-2#)
 * Account: bouldercompsci

2. Navigate to S3
 * [Amazon S3 Moodle Bucket](https://s3.console.aws.amazon.com/s3/buckets/cu-cs-moodle-backups/?region=us-east-2&tab=overview)

3. Click the Create Folder Button

4. Privide a new folder name (YYYY-semester)

5. Click Save

6. Connect to a Moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php
kubectl exec -it [PHP POD NAME] /bin/bash
```

7. Move into the Moodle root
```
cd /srv/moodle/code
```

8. Change into the Moodle user
```
su -s /bin/bash www-data
```

9. Create backup location
```
mkdir /tmp/backups
```

10. Identify the category ID to be backed-up
```
/srv/moodle/moosh/moosh.php category-list
```

11. Add the backup script (replace [CATEGORY_ID] with the desired number from step 10)
```
cat > /tmp/backup.sh << EOF
for COURSEID in \$( /srv/moodle/moosh/moosh.php course-list -c [CATEGORY_ID] -f id | grep  \" | tail -n +2 | tr -d \" ); do echo Processing \$COURSEID; /usr/bin/php /srv/moodle/code/admin/cli/backup.php --courseid=\$COURSEID --destination=/tmp/backups; rm -rf /srv/moodle/data/temp/backup/* ; rm -rf /srv/moodle/data/trashdir ; done
EOF
```

12. Start the backup
```
chmod 700 /tmp/backup.sh
nohup /tmp/backup.sh > /tmp/nohup.txt &
```

13. Monitor progress until completion
```
tail -f /tmp/nohup.txt
```


14. Copy backups to Amazon S3
* See [PHP S3 Upload script](#php-s3-upload-script)

15. Remove local backups
```
rm -rf /tmp/backups
rm /tmp/backup.sh
```

## Install S3 PHP v3

1. Go to AWS S3 PHP installation instructions
[AWS S3 PHP Installation Instructions](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/getting-started_installation.html)

2. Download the ZIP file from under the Installing by Using the ZIP file header

3. Open a terminal window

4. Copy the ZIP file into a new folder
```
cd /tmp
mkdir awsphp
cd awsphp
mv aws.zip /tmp/awsphp
```

5. Extract the ZIP
```
cd awsphp
unzip aws.zip
```

6. Remove the ZIP
```
rm aws.zip
```

7. Recompress ZIP as Gzipped TAR from the parent directory
```
cd ..
tar -cvzf /tmp/awsphp.tgz awsphp
```

8. Copy file to Moodle PHP
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php
kubectl cp awsphp.tgz [PHP POD NAME]:/tmp/
```

9. Login to a Moodle PHP container
```
kubectl exec -it [PHP POD NAME] /bin/bash
```

10. Decompress archive and remove
```
cd /tmp
gzip -dc awsphp.tgz | tar -xvf -
rm awsphp.tgz
```


## PHP S3 Upload script

Prerequisites: [Install S3 PHP v3](#install-s3-php-v3)

1. Connect to a Moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php
kubectl exec -it [PHP POD NAME] /bin/bash
```

1. Create backup script (**Key and secret must be replaced. See the vault for actual values.**)
```
cat > /tmp/s3backup.php << EOF
#!/usr/bin/env php
<?php
require '/tmp/awsphp/aws-autoloader.php';
use Aws\Common\Exception\MultipartUploadException;
use Aws\S3\MultipartUploader;
use Aws\S3\S3Client;
// f = filename (in current directory)
// t = Term (e.g. 2019-spring, 2019-summer) folder name
\$arguments = getopt('f:t:');
\$bucket = 'cu-cs-moodle-backups';
\$keyname = \$arguments['t'] . '/' . \$arguments['f'];                      
\$s3 = new S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'credentials' => [
        'key'    => 'ACCESS_KEY_ID',
        'secret' => 'SECRET_ACCESS_KEY'
    ]
]);
// Prepare the upload parameters.
\$uploader = new MultipartUploader(\$s3, \$arguments['f'], [
    'bucket' => \$bucket,
    'key'    => \$keyname,
    'ACL'    => 'private'
]);
// Perform the upload.
try {
    \$result = \$uploader->upload();
    echo "Upload complete: {\$result['ObjectURL']}" . PHP_EOL;
} catch (MultipartUploadException \$e) {
    echo \$e->getMessage() . PHP_EOL;
}
?>
EOF
```

3. Run backup script for each backup
```
/tmp/s3backup.php -t [TERM] -f [BACKUP_FILE_NAME]
```
Term from Backup Moodle courses step #4

4. Login to S3
5. Navigate to the term's upload location
* The number of files is listed at the right side of the header and footer of the file list table.

6. Confirm all backups are present
    1. Login to Moodle
    2. Click Site adminstration
    3. Click the Courses tab
    4. Click Manage courses and categories
    5. Under the *Course categories* see the number at the end of the category entry and compare to the number of uploaded files in the S3 bucket's term folder.

7. Change Storage Class on all backups to Glacier
    1. Checkbox the term folder
    2. From the Action drop-down click Change storage class
    3. Click the Glacier radio-button
    4. Click Save

8. Delete the backups
```
rm -rf /tmp/backups
```
9. Delete supporting script
```
rm /tmp/s3backup.php
rm /tmp/backup.sh
```

## PHP S3 Download script

Prerequisites: [Install S3 PHP v3](#install-s3-php-v3)

1. Connect to a Moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php
kubectl exec -it [PHP POD NAME] /bin/bash
```

1. Create backup script (**Key and secret must be replaced. See the vault for actual values.**)
```
cat > /tmp/s3download.php << EOF
#!/usr/bin/env php
<?php
require '/tmp/awsphp/aws-autoloader.php';
use Aws\S3\S3Client;  
use Aws\Exception\AwsException;
\$arguments = getopt('f:t:');
if (count(\$arguments) != 2) {
    echo "\n\nGetObject.php -t <yyyy-term> -f <filename>\n\n";
    exit();
}
\$bucket = 'cu-cs-moodle-backups';
\$keyname = \$arguments['t'] . '/' . \$arguments['f'];                      
try {
  \$s3 = new S3Client([
    'version' => 'latest',
    'region'  => 'us-west-2',
    'credentials' => [
        'key'    => 'ACCESS_KEY_ID',
        'secret' => 'SECRET_ACCESS_KEY'
    ]
  ]);
	\$result = \$s3->getObject(array(
	        'Bucket' => \$bucket,
	        'Key' => \$keyname,
	        'SaveAs' => \$arguments['f']
	    ));
} catch (S3Exception \$e) {
    echo \$e->getMessage() . "\n";
}
?>
EOF
```
3. Make the script executable
```
chmod 700 /tmp/s3download.php
```

4. Run backup script for each backup
```
/tmp/s3download.php -t [TERM] -f [BACKUP_FILE_NAME]
```


## Delete a category with courses

**WARNING: This will remove a category and all the nested courses. Ensure the courses have been backed up and use with extreme caution**

1. Connect to a moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep php
kubectl exec -it [PHP POD NAME] /bin/bash
```

2. Switch to the Moodle user
```
su -s /bin/bash www-data
```

3. Move into the Moodle code base
```
cd /srv/moodle/code
```

4. List existing categories
```
/srv/moodle/moosh/moosh.php category-list
```

5. Delete the desired category (Replace [CATEGORY_ID] the desired category number from step 4)
```
/srv/moodle/moosh/moosh.php category-delete [CATEGORY_ID]
```

## Expand Moodle data disk

1. Using GCP console or gcloud CLI, increase the size of the *moodle-data-disk-ssd*
2. Login to the NFS pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep nfs
kubectl exec -it [PHP POD NAME] /bin/bash
```
3. Find the moodle data disk device
```
df -h | grep exports
```
4. Resize data disk (replace [DEVICE] with value from step 3)
```
resize2fs /dev/[DEVICE]
```


## REMUI Update Nag Removal
Use this Custom CSS code in Custom CSS section inside Edwiser RemUI general settings-

```
.update-nag {
display:none
}
```

## Manual SSL Certificate Refresh (Let's Encrypt)
1. Connect to a Moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep letsencrypt
kubectl exec -it [POD NAME] /bin/bash
```
2. Move into the script directory
```
cd /opt
```

3. Execute the update script
```
./gen_le_cert.sh
```

4. Reload the moodle webpage and confirm the new certificate is active

Alternatively, test it with a service like [SSL Labs](https://www.ssllabs.com/ssltest/)

## Problem: Email sends, but is not delivered

There could be many possible problems, and here we will look at how to determine the root cause.

1. Connect to a Moodle pod
```
gcloud config set project emerald-agility-749
kubectl config use-context gke_emerald-agility-749_us-west1-a_production
kubectl config set-context gke_emerald-agility-749_us-west1-a_production --namespace moodle-prod
kubectl get pod | grep smtp
kubectl exec -it [POD NAME] /bin/sh
```

2. Find the deferred mail spool
```
cd /var/spool/postfix/defer
```
3. Select a queue
```
ls
cd [AnyQueue]
ls
cat [AnyMessage]
```

4. Examine the message file contents for **reason=**

5. Check external IP
```
cd /tmp
wget https://ipchicken.com/
cat index.html  | grep -A 1 Address
rm index.html
```
This address should be 104.198.9.1 (if not assign that IP to the node Moodle node)

## Alert! License is not activated , please activate the license in RemUI settings.
1. Login to Moodle
2. Navigate to Edwiser RemUI settings > License Status
3. Click the Activate License button

If that does not resolve the problem do the following and contact EdWiser
1. Navigate to Edwiser RemUI settings > General settings
2. In the **Custom CSS section** add the following
```
.license-nag {
  display:none;
}
```
3. Click the **Save changes** button at the bottom of the page

## Broken question (missing prototype 'xxxxxxxxx'). Cannot be run
1. Login to Moodle
2. Navigate to the class and quiz generating the error message
3. Click the Gear icon
4. Click Question Bank
5. From the **Select a category** drop-down, Select CR_PROTOTYPES
6. Delete any duplicates (usually not edited by an admin)
7. Repeat steps using LOCAL_PROTOTYPES form the drop-down
