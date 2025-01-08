# mail-cow
If you want to keep your backups on the same server, you can store them in a dedicated directory outside of your Mailcow installation to ensure they are safe during any accidental operations on the Mailcow directory. Here’s how to modify the process:
*Backup and Rollback Process (Same Server)

Backup Process (Same Server)

1. Create a Dedicated Backup Directory
   Create a separate directory on the same server for storing backups:

   mkdir -p /path/to/local/backup


   Example:

   mkdir -p /backups/mailcow


2. Run the Backup Scrip
   Execute the backup script and specify the backup directory:

   cd /opt/mailcow-dockerized/helper-scripts/
   ./backup_and_restore.sh backup all


3. Move Backups to the Backup Directory
   After the backup completes, move the generated files to the dedicated backup location:

   mv /opt/mailcow-dockerized/helper-scripts/backup/* /path/to/local/backup/
   

4. Automating Local Backups

   crontab -e

   Add the following line (e.g., daily at midnight):00.00 Midnight

   0 0 * * * cd /opt/mailcow-dockerized/helper-scripts/ && ./backup_and_restore.sh backup all && mv /opt/mailcow-dockerized/helper-scripts/backup/* /path/to/local/backup/
   

5. Verify the Backup
   List the backup files in your local backup directory to ensure the process worked:
   
   ls -lh /path/to/local/backup/
   

Rollback (Restore) Process

1. Stop Mailcow Service  
   Stop the Mailcow services before restoring:
   
   cd /opt/mailcow-dockerized/
   docker-compose down
   

2. Copy Backup Files to Original Location
   Move the desired backup files back to the Mailcow helper-scripts directory:
   
   cp -r /path/to/local/backup/* /opt/mailcow-dockerized/helper-scripts/backup/
   

3. Restore Backup  
   Use the restore script to recover from the backup:
   
   cd /opt/mailcow-dockerized/helper-scripts/
   ./backup_and_restore.sh restore
   

4. Start Mailcow Services
   After restoring, start the Mailcow services:
   
   cd /opt/mailcow-dockerized/
   docker-compose up -d
   

5. Test the Restore 
   Verify the functionality of Mailcow by checking the web interface, emails, and other services.

Additional Notes
- Ensure the backup directory has sufficient disk space.
- For added security, set proper permissions on the backup directory:
  
  chmod -R 700 /path/to/local/backup
  chown -R root:root /path/to/local/backup
  



This ensures your backups are stored securely on the same server without relying on remote storage.
