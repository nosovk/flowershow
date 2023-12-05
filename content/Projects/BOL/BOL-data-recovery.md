# In case of some data loss in the database or the file storage

1. Notify the IT partner about the ongoing incident
2. Wait until the IT partner responds
3. Read the documentation
   1. https://wal-g.readthedocs.io/
   2. https://restic.readthedocs.io/en/latest/
4. Stop all the services [remote VM]
    1. `systemctl stop traefik`
    2. `systemctl stop nginx`
    3. `systemctl stop php-fpm`
    4. `systemctl stop redis`
    5. `systemctl stop mariadb`
    6. `systemctl stop supercronic`
5. Copy the existing data to the temporary remote location [remote VM]
6. Download the binary files [local environment]
    1. restic
        1. https://github.com/restic/restic/releases
        2. `wget https://github.com/restic/restic/releases/download/v0.16.2/restic_0.16.2_linux_amd64.bz2`
    2. walg
        1. https://github.com/wal-g/wal-g/releases
        2. `wget https://github.com/wal-g/wal-g/releases/download/v2.0.1/wal-g-mysql-ubuntu-20.04-amd64.tar.gz`
7. Unpack the binary files [local environment]
    1. `bzip2 -d restic_0.16.2_linux_amd64.bz2`
    2. `tar -zxvf wal-g-mysql-ubuntu-20.04-amd64.tar.gz`
8. Remove archives [local environment]
    1. `rm wal-g-mysql-ubuntu-20.04-amd64.tar.gz`
9. Add execution permissions [local environment]
    1. `chmod +x restic_0.16.2_linux_amd64`
    2. `chmod +x wal-g-mysql-ubuntu-20.04-amd64`
10. Rename the binary files [local environment]
    1. `mv wal-g-mysql-ubuntu-20.04-amd64 wal-g`
    2. `mv restic_0.16.2_linux_amd64 restic`
11. Move the binaries to the directory, which included to the PATH [local environment]
    1. `sudo mv wal-g /usr/local/bin/wal-g`
    2. `sudo mv restic /usr/local/bin/restic`
12. Bootstrap secrets to the shell [local environment] {it is necessary to the replace the <SECRET> with an actual value}
    1. `export AWS_ACCESS_KEY_ID=<SECRET>`
    2. `export AWS_SECRET_ACCESS_KEY=<SECRET>`
    3. `export RESTIC_PASSWORD=<SECRET>`
13. Create necessary directories [local environment]
    1. `mkdir -p /tmp/restic/classes /tmp/restic/config /tmp/restic/var /tmp/restic/versions /tmp/restic/recyclebin /tmp/walg/mariadb`
14. Bootstrap env vars to the shell [local environment]
    1. `export RESTIC_REPOSITORY_CLASSES='s3://fra1.digitaloceanspaces.com/bol-bol-pim-application-classes'`
    2. `export RESTIC_RESTORE_DESTINATION_CLASSES='/tmp/restic/classes'`
    3. `export RESTIC_REPOSITORY_CONFIG='s3://fra1.digitaloceanspaces.com/bol-bol-pim-application-config'`
    4. `export RESTIC_RESTORE_DESTINATION_CONFIG='/tmp/restic/config'`
    5. `export RESTIC_REPOSITORY_VAR='s3://fra1.digitaloceanspaces.com/bol-bol-pim-application-var'`
    6. `export RESTIC_RESTORE_DESTINATION_VAR='/tmp/restic/var'`
    7. `export RESTIC_REPOSITORY_VERSIONS='s3://fra1.digitaloceanspaces.com/bol-bol-pim-application-versions'`
    8. `export RESTIC_RESTORE_DESTINATION_VERSIONS='/tmp/restic/versions'`
    9. `export RESTIC_REPOSITORY_RECYCLEBIN='s3://fra1.digitaloceanspaces.com/bol-bol-pim-application-recyclebin'`
    10. `export RESTIC_RESTORE_DESTINATION_RECYCLEBIN='/tmp/restic/recyclebin'`
    11. `export WALG_S3_PREFIX='s3://bol-bol-pim-walg-mysql-storage'`
    12. `export AWS_ENDPOINT='fra1.digitaloceanspaces.com'`
    13. `export AWS_S3_FORCE_PATH_STYLE='true'`
    14. `export WALG_STREAM_RESTORE_COMMAND="mbstream -x -C /tmp/walg/mariadb"`
15. Restore the file backups one by one [local environment]
    1. `restic --repo <REPO> snapshots`
    2. `restic --repo <REPO> stats --mode raw-data`
    3. `restic -r <REPO> restore latest --target <RESTORE_DIR>`
    4. example
        1. classes
            1. `restic --repo ${RESTIC_REPOSITORY_CLASSES} snapshots`
            2. `restic --repo ${RESTIC_REPOSITORY_CLASSES} stats --mode raw-data`
            3. `restic -r ${RESTIC_REPOSITORY_CLASSES} restore latest --target ${RESTIC_RESTORE_DESTINATION_CLASSES}`
        2. config
            1. `restic --repo ${RESTIC_REPOSITORY_CONFIG} snapshots`
            2. `restic --repo ${RESTIC_REPOSITORY_CONFIG} stats --mode raw-data`
            3. `restic -r ${RESTIC_REPOSITORY_CONFIG} restore latest --target ${RESTIC_RESTORE_DESTINATION_CONFIG}`
        3. var
            1. `restic --repo ${RESTIC_REPOSITORY_VAR} snapshots`
            2. `restic --repo ${RESTIC_REPOSITORY_VAR} stats --mode raw-data`
            3. `restic -r ${RESTIC_REPOSITORY_VAR} restore latest --target ${RESTIC_RESTORE_DESTINATION_VAR}`
        4. versions
            1. `restic --repo ${RESTIC_REPOSITORY_VERSIONS} snapshots`
            2. `restic --repo ${RESTIC_REPOSITORY_VERSIONS} stats --mode raw-data`
            3. `restic -r ${RESTIC_REPOSITORY_VERSIONS} restore latest --target ${RESTIC_RESTORE_DESTINATION_VERSIONS}`
        5. recyclebin
            1. `restic --repo ${RESTIC_REPOSITORY_RECYCLEBIN} snapshots`
            2. `restic --repo ${RESTIC_REPOSITORY_RECYCLEBIN} stats --mode raw-data`
            3. `restic -r ${RESTIC_REPOSITORY_RECYCLEBIN} restore latest --target ${RESTIC_RESTORE_DESTINATION_RECYCLEBIN}`
16. Install the mariadb-backup package [local environment]
    1. `sudo apt install mariadb-backup`
17. Restore the database backup [local environment]
    1. `mkdir -p /tmp/walg/mariadb`
    2. `wal-g backup-list --pretty --detail`
    3. `wal-g backup-fetch LATEST`
18. Move the remaining data to the temporary location [remote VM]
    1. /opt/app/var/classes
    2. /opt/app/var/config
    3. /opt/app/public/var
    4. /opt/app/var/versions
    5. /opt/app/var/recyclebin
    6. /var/lib/mysql
19. Copy the restored data to the remote VM [remote VM]
20. Fix permissions for the restored data [remote VM]
21. Start the services [remote VM]
    1. `systemctl start traefik`
    2. `systemctl start nginx`
    3. `systemctl start php-fpm`
    4. `systemctl start redis`
    5. `systemctl start mariadb`
    6. `systemctl start supercronic`
22. Remove the temporary data [local environment]