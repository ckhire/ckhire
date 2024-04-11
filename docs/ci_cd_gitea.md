## CI/CD With Gitea
Yes Gitea provides simpler and completely self managed CI/CD. Just follow below steps to set-up simple your `Hello-World` runners

1. Get Gitea runner:
   Gitea runner is an independent runner program know as `Act`. It is recommended to install it on machines where you want your programs to build. It has support for dockers too. Here I am focusing on runner without docker.
   Here I have installed on same machine as my server. For server just check previous article.  
   - Open `https://dl.gitea.com/act_runner/` in your browser and select the folder suitable for your machine. Here I have downloaded `act_runner-0.2.6-linux-amd64`.
   - > chmod +x act_runner     # Making our file executable

2. Configure Act Runner:  
   There are lot of configurations to control runner. To generate configuration file enter below command
     > ./act_runner generate-config > config.yaml
     
    As you can see it generates the config file on terminal thus we directed it to our `config.yaml`. You can give some other name.
     
3. Connecting our Gitea Server with Act Runner:

  Gitea Server provides registration process in which it provides token which we need to give to our Act Runner. So that Server knows about the Act Runner which is polling.  
  Yes by polling. Our Act Runner will continuously keeps pooling to Server (because our Runner IP is not public to provide hook mechanism). On the other hand as soon as the event as per given in CI/CD script gets triggered server will look for all regiterd runners and will provide apporpriate information to the matched runner.  
  We will soon see this practically and unlike above steps we have images for this section. Before that one important thing 🤔  
  There are **3** levels of execution for 🏃.  
  1. Instance Level  :--> Don't get 😕. As we know our server is an instance. We may have multiple instances or you may 🤔 like single server instance may have many organization and ..yes 🏁 runners shared across multiple organizations.  
  2. Organization Level :--> Runners setup at this level are inherited by all repository within organization and any repo can use it.  
  3. Repository Level  :--> These will get triggerd for events of specific repository. One can say each repository has its own or shares same runner.

🤔 How to get token for each of this levels ❔

## For Repository level:  
![Screenshot from 2024-04-11 00-13-23](https://github.com/ckhire/ckhire/assets/60615631/be2991bf-08b2-4222-a96e-386577504af8)  

Now under Actions section from left panel  

![Screenshot from 2024-04-11 00-13-41](https://github.com/ckhire/ckhire/assets/60615631/c29df0ff-3efb-45e1-b3f2-d009fd57ccd1)  

Click runner and then `Create New Runner`. Just copy the token. In my case I already had one.  

![Screenshot from 2024-04-11 00-13-54](https://github.com/ckhire/ckhire/assets/60615631/1edba47a-6d19-4bff-85ca-9dca0cbbcf30)  

## For Organization level:  
![Screenshot from 2024-04-11 00-15-53](https://github.com/ckhire/ckhire/assets/60615631/1766885e-2587-43e4-91ba-0535c42c6b51)  

![Screenshot from 2024-04-11 00-16-02](https://github.com/ckhire/ckhire/assets/60615631/32df5016-9897-4b10-898d-350f28f0454d)  

Here  you may not see any runner because I have not register my token for `CkhireAI` organization.
![Screenshot from 2024-04-11 00-16-10](https://github.com/ckhire/ckhire/assets/60615631/19defd5f-8979-425c-8ee4-c988fa8ba60c)


❔ ⏭️ How to feed this to Act Runner instance knows as Registration of Act Runner

I] 🖨️ Interactive way
> ./act_runner --config config.yaml register

> The Gitea instance URL, like https://gitea.com/ or http://192.168.8.8:3000/

> _Paste the token copied above_

> _Runner Name_ It chooses host machine name if filled blank

> **_Runner lable name_**  #This is paired like `os_name-version_name` like ubuntu-20.04. This is very important as it will pick this lable from the ci/cd script provided in repo and as per the lable it will match against the registered one using this lable and will interact with runner as per given script. Below are screenshots for ⤵️

![Screenshot from 2024-04-12 00-02-34](https://github.com/ckhire/ckhire/assets/60615631/2193c1da-d7a8-46c9-8561-80a29a26da51)

![Screenshot from 2024-04-12 00-02-42](https://github.com/ckhire/ckhire/assets/60615631/8bf2c90b-8317-40da-9725-d6ffa25763c1)


✔️ ❎ We are not done yet. 🤔 🧐 Below is the simple `build.yaml` required in `.gitea/workflows` folder in the `root` 📁 of your repository.  
```yaml
name: Trial

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-20.04
    defaults:
      run:
        working-directory: /home/ckhire/repos/tracking
    steps:
      - name: listing
        run: ls -lrt

      - name: Checkout repo
        run: git pull origin main
```

![Screenshot from 2024-04-12 00-03-46](https://github.com/ckhire/ckhire/assets/60615631/12d8594c-7308-47a2-8570-934e5ee71992)

![Screenshot from 2024-04-12 00-03-55](https://github.com/ckhire/ckhire/assets/60615631/545fd975-ffe6-47cb-80f2-c11d08b476ad)

![Screenshot from 2024-04-12 00-04-18](https://github.com/ckhire/ckhire/assets/60615631/bf5327d0-d506-4384-bf0a-0645affef0e6)

![Screenshot from 2024-04-12 00-05-04](https://github.com/ckhire/ckhire/assets/60615631/3324a523-1ee8-4425-97a8-170dc6eef3b9)


