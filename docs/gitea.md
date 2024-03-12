# Bridging the Gap: Bringing GitHub-like Collaboration On-Premises

## Missing Fun:

Being part of a long-standing organization with a legacy in the Vision AI domain is a source of immense pride. However, amidst the excitement of tackling new challenges, the method of accomplishing tasks and maintaining clear communication with stakeholders and managers often gets lost. This can lead to confusion among team members, impacting productivity and morale. 

Missing GitHub in such scenarios is palpable, especially when the development philosophy emphasizes finalizing code before committing, leaving individual performers struggling to navigate through the chaos.

## Search for On-premise GitHub:

While GitLab emerges as a viable alternative, its extensive feature set and organizational implications make it a significant decision. Exploring options led me to Gitea, a lightweight solution that aligns perfectly with my needs. 

The ease of installation, coupled with essential features like Kanban boards, issue tracking, custom labels, and time tracking, makes Gitea a lifesaver. Its simplicity and efficiency allow for seamless collaboration within the team, enabling clear issue definition, sign-offs from team leads, and productive discussions on code changes.

### Installation Steps:

1. Choose Ubuntu for comfortable installation.
   > wget -O gitea https://dl.gitea.com/gitea/1.21.7/gitea-1.21.7-linux-amd64
2. Copy the gitea to `usr/local/bin/gitea/` by creating gitea in bin folder
3. I liked to execute the gitea under by user thus I am skipping creation of new user to launch this as service under specific user only.
4. Create directory structure as required by gitea as follows
   > mkdir -p /var/lib/gitea/{custom,data,log}  
     sudo chown -R `ckhire`:`ckhire` /var/lib/gitea/  
     chmod -R 750 /var/lib/gitea/  
     mkdir /etc/gitea  
     chown `root`:`ckhire` /etc/gitea  
     chmod 770 /etc/gitea  
   > Just use your user_name instead of `ckhire`
5. Create new enviornment variable
   > export GITEA_WORK_DIR=/var/lib/gitea/

6. Create service to run gitea server as service for this I picked file from [here](https://github.com/go-gitea/gitea/blob/release/v1.21/contrib/systemd/gitea.service)
7. Below are the changes I made in the above service file
   > [Service]  
   RestartSec=2s  
   Type=simple  
   User=ckhire  
   Group=ckhire  
   WorkingDirectory=/var/lib/gitea/  
   Environment=USER=ckhire HOME=/home/ckhire GITEA_WORK_DIR=/var/lib/gitea
  
_Kindly Note: If in case service doesn't start follow below command and proced otherwise you can continue with step 9_
8. > cd /usr/local/bin/gitea  
   ./gitea-1.21.4-linux-amd64 web -c /etc/gitea/app.ini  
   > Output looks similar to below
   
9. Open web browser and enter `localhost:3000` as its the default port for gitea.
7. Install with the same user as your own.
8. Follow the official installation guide for straightforward setup.
9. Enjoy features like Kanban boards, issue tracking, pull requests, and time tracking.
10. Benefit from clear communication and efficient collaboration within your team.

In conclusion, Gitea fills the void left by GitHub, providing a reliable on-premise solution for code collaboration without the overhead of unnecessary features. Its ease of use and essential functionality make it a valuable asset for navigating complex development workflows while ensuring clarity and efficiency in communication.
