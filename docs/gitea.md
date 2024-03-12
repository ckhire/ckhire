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

   ![Screenshot from 2024-03-13 00-17-00](https://github.com/ckhire/ckhire/assets/60615631/bcaeb76d-8cdc-4264-8c80-6b0c6a8ed533)

   
9. Open web browser and enter `localhost:3000` as its the default port for gitea. You should see the page as below

![Screenshot from 2024-03-06 23-20-15](https://github.com/ckhire/ckhire/assets/60615631/78e3d170-a28b-4b9d-94fb-8e4029bec984)

![Screenshot from 2024-03-06 23-20-37](https://github.com/ckhire/ckhire/assets/60615631/f65fd3dc-0d87-44a0-b990-5a50fd23903f)

_Kindly choose `SQLitev3` as the database for faster installation and better performance for beginner. Replace user name with yours and same for the other fields like repository directory for server, its logs, http request port to listen_  
10. Finally after installation and starting the service you should see below page  

   ![Screenshot from 2024-03-13 00-15-07](https://github.com/ckhire/ckhire/assets/60615631/ceb33aad-12bd-47d2-8e8c-e92241a418e8)

11. Just register your user name if not already done for this `local server`.You will see below screen post login

     ![Screenshot from 2024-03-13 00-25-35](https://github.com/ckhire/ckhire/assets/60615631/ad5553d7-8a0e-4d56-9cca-9569b73f5658)


## Excited to start with gitea

1. Just create a repository by clicking `+` icon  

![image](https://github.com/ckhire/ckhire/assets/60615631/a861a455-f02a-4353-be69-c68635d46a03)

2. Just fill the information as required

   ![image](https://github.com/ckhire/ckhire/assets/60615631/36cb9e29-27e1-4c19-ad72-6d57ad56016c)

3. Rest everything is like github. I would 💌 to move towards Automated Kanban board by clicking on the `Project` from below screen

   ![image](https://github.com/ckhire/ckhire/assets/60615631/d4385052-0a33-4f2c-8b5f-f3fcebcc7f8c)
   
🥇 In my case I already create `Tracking` board  

   ![image](https://github.com/ckhire/ckhire/assets/60615631/01962990-fcbe-4739-8d9a-b23d9ef4a69c)

👌 I was more excited when I got to see `milestones`, `labels`. For `issue` I have **Time Tracker** ⏰ never had this before. I loved the issue page here is quick sample view of mine page

   ![image](https://github.com/ckhire/ckhire/assets/60615631/aa3d401c-72d1-478f-9670-a491fe1ff64e)


In conclusion, Gitea fills the void left by GitHub, providing a reliable on-premise solution for code collaboration without the overhead of unnecessary features. Its ease of use and essential functionality make it a valuable asset for navigating complex development workflows while ensuring clarity and efficiency in communication.

_Note:_ For quick step by step usage of gitea I would soon bring simple video series showing different functionality and its usage.  

🙂 Thanks You ! 🙏 Your feedback is much 👏
