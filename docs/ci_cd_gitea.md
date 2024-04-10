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
